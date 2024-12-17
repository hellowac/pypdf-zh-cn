# 从 PDF 中提取文本  

你可以从 PDF 中提取文本：  

```python  
from pypdf import PdfReader  

reader = PdfReader("example.pdf")  
page = reader.pages[0]  
print(page.extract_text())  

# 仅提取正向文本  
print(page.extract_text(0))  

# 提取正向和左旋 90 度的文本  
print(page.extract_text((0, 90)))  

# 以固定宽度格式提取文本，尽量贴合源 PDF 的渲染布局  
print(page.extract_text(extraction_mode="layout"))  

# 提取文本时保留水平位置，但去除多余的垂直空白行（删除空行和“仅空白”行）  
print(page.extract_text(extraction_mode="layout", layout_mode_space_vertically=False))  

# 调整水平间距  
print(page.extract_text(extraction_mode="layout", layout_mode_scale_weight=1.0))  

# 默认排除旋转文本，也可以包括旋转文本（如下所示）  
print(page.extract_text(extraction_mode="layout", layout_mode_strip_rotated=False))  
```  

参考 {func}`~pypdf._page.PageObject.extract_text` 了解更多细节。  

## 使用访问器  

你可以使用访问器函数控制需要处理和提取页面的哪个部分。访问器函数将在每个操作符或文本片段时被调用。  

`extract_text` 方法中 `visitor_text` 参数接受的函数有以下五个参数：  
- **text**：当前文本（尽可能长，可能是完整的一行）。  
- **user_matrix**：用户坐标空间转换矩阵（也称为 CTM）。  
- **tm_matrix**：文本坐标空间的当前矩阵。  
- **font-dictionary**：字体字典的完整内容。  
- **font-size**：文本的字体大小（基于文本坐标空间）。  

矩阵有六个参数：前四个参数定义旋转/缩放，后两个参数定义水平/垂直平移。  
推荐使用 `user_matrix`，因为它考虑了所有变换。  

### 注意事项  
- 根据 PDF 1.7 或 PDF 2.0 规范 §8.3.3，`user_matrix` 适用于文本空间、图像空间、表单空间和模式空间。  
- 如果需要完整的从文本到用户空间的转换，可以使用函数 {func}`~.pypdf.mult`：  
  ```python  
  txt2user = mult(tm, cm)  
  ```  
  字体大小是原始文本大小，受到 `user_matrix` 的影响。  

字体字典在字体未知的情况下可能为 `None`。如果不为 `None`，可能包含键 "/BaseFont"，值为 "/Arial,Bold"。  

**注意**：在复杂文档中，计算的位置信息可能较难处理（例如在多表单转换到页面用户空间的情况下）。  

`visitor_operand_before` 参数接收的函数有以下四个参数：  
- 操作符  
- 操作数  
- 当前变换矩阵  
- 文本矩阵  

### 示例 1：忽略页眉和页脚  

以下示例读取[此 PDF 文档](https://github.com/py-pdf/pypdf/blob/main/resources/GeoBase_NHNC1_Data_Model_UML_EN.pdf)的第四页的正文，忽略页眉（y > 720）和页脚（y < 50）。  

```python  
from pypdf import PdfReader  

reader = PdfReader("GeoBase_NHNC1_Data_Model_UML_EN.pdf")  
page = reader.pages[3]  

parts = []  


def visitor_body(text, cm, tm, font_dict, font_size):  
    y = cm[5]  
    if 50 < y < 720:  
        parts.append(text)  


page.extract_text(visitor_text=visitor_body)  
text_body = "".join(parts)  

print(text_body)  
```  

### 示例 2：将矩形和文本提取到 SVG 文件中  

以下示例将[此 PDF 文档](https://github.com/py-pdf/pypdf/blob/main/resources/GeoBase_NHNC1_Data_Model_UML_EN.pdf)的第三页转换为 [SVG 文件](https://en.wikipedia.org/wiki/Scalable_Vector_Graphics)。  

这种 SVG 导出可以帮助理解页面的布局和内容。  

```python  
from pypdf import PdfReader  
import svgwrite  

reader = PdfReader("GeoBase_NHNC1_Data_Model_UML_EN.pdf")  
page = reader.pages[2]  

dwg = svgwrite.Drawing("GeoBase_test.svg", profile="tiny")  


def visitor_svg_rect(op, args, cm, tm):  
    if op == b"re":  
        (x, y, w, h) = (args[i].as_numeric() for i in range(4))  
        dwg.add(dwg.rect((x, y), (w, h), stroke="red", fill_opacity=0.05))  


def visitor_svg_text(text, cm, tm, fontDict, fontSize):  
    (x, y) = (cm[4], cm[5])  
    dwg.add(dwg.text(text, insert=(x, y), fill="blue"))  


page.extract_text(  
    visitor_operand_before=visitor_svg_rect, visitor_text=visitor_svg_text  
)  
dwg.save()  
```  

生成的 SVG 坐标系是从底部开始的，因为 PDF 和 SVG 的坐标系不同。  

**注意**：在复杂的 PDF 文档中，访问器函数提供的坐标可能会不准确。  

---

## 为什么文本提取如此困难  

### 不清晰的目标  

从 PDF 中提取文本可能非常棘手。在许多情况下，如何定义期望的结果并不明确：  

1. **段落**：提取的段落是否应该保留原 PDF 的换行，还是作为一个整体？  
2. **页码**：是否应将页码包含在提取的文本中？  
3. **页眉和页脚**：类似于页码——是否需要提取？  
4. **大纲**：是否需要提取大纲？  
5. **格式**：如果文本是 **加粗** 或 *斜体*，是否需要保留格式？  
6. **表格**：是否应跳过表格提取？仅提取文本，还是以 Markdown 或 HTML 表格形式保留结构？如何处理合并单元格？  
7. **说明文字**：是否需要提取图表或表格的说明文字？  
8. **连字**：例如，Unicode 符号 [U+FB00](https://www.compart.com/de/unicode/U+FB00) 表示两个小写字母 "f" 的单个符号“ﬀ”。提取时应显示为单个符号还是分开为 "ff"？  
9. **SVG 图像**：是否提取 SVG 图像中的文本部分？  
10. **数学公式**：是否需要提取公式，包括下标和嵌套分数？  
11. **空白字符**：3 厘米的垂直空白应提取为多少行？3 厘米的水平空白应提取为多少空格？是否需要提取制表符？  
12. **脚注**：跨页提取文本时，脚注应显示在什么位置？  
13. **超链接和元数据**：是否需要提取？应该以何种格式和位置呈现？  
14. **线性化**：如果段落中间有浮动图形，是先完成段落还是将图形文本插入段落中间？  

### 实现难度  

有些问题在逻辑上可行，但由于 PDF 的存储方式，实际上很难实现：  

1. **表格**：通常表格只是绝对定位的文本，最糟情况下每个字符都可能是绝对定位的，这使得识别列和行变得困难。  
2. **图像**：有些 PDF 的文本是以图像形式存储的，这时无法复制文本。还有一些 PDF 包含图像和背景文本层，这通常发生在扫描文档时。尽管 OCR 技术已经相当先进，但仍有可能失败。pypdf 不是 OCR 软件，无法检测这些失败或从图像中提取文本。  

对于 pypdf 能处理的文本提取问题，如果发现 Bug，请分享相关 PDF，以便改进！  

### 缺失的语义层

PDF 文件格式的核心目标是实现预期的打印视觉效果，而非解析内容。PDF 文件中没有语义层。  

具体而言，PDF 文件中不包含有关页眉、页脚、页码、表格和段落的信息。虽然人类可以通过启发式方法进行合理推测，但无法确定。  

这是 PDF 文件格式的局限性，而非 pypdf 的问题。  

可以通过对 PDF 文档应用机器学习来优化启发式方法，但这不是 pypdf 的职责。然而，pypdf 可以作为数据源，提取相关信息以供机器学习系统使用。  

---

### 空白字符

PDF 格式是为打印设计的，而非机器可读的格式。PDF 文档中的文本是绝对定位的，这意味着每个字符都可能在页面上有自己的位置。  

例如，文本：  
> This is a test document by Ethan Nelson.  

可以表示为：  
> [(This is a )9(te)-3(st)9( do)-4(cu)13(m)-4(en)12(t )-3(b)3(y)-3( )9(Et)-2(h)3(an)4( Nels)13(o)-5(n)3(.)] TJ  

其中的数字表示垂直空间的调整。这种 PDF 内部表示方式使得正确解析空白字符变得非常困难。  

更多信息：  

- [issue #1507](https://github.com/py-pdf/pypdf/issues/1507)  
- [PDF 内容流中文本对象的负数解释](https://stackoverflow.com/a/28203655/562769)  
- Mark Stephens: [理解 PDF 文本对象](https://blog.idrsolutions.com/understanding-pdf-text-objects/)，2010 年。  

---

## OCR 与文本提取

光学字符识别 (OCR) 是从图像中提取文本的过程。执行此任务的软件称为 **OCR 软件**。  
[tesseract OCR 引擎](https://github.com/tesseract-ocr/tesseract) 是最著名的开源 OCR 软件之一。  

pypdf 并非 OCR 软件。  

---

### 数字生成与扫描的 PDF 文件

PDF 文档可以包含图像和文本。PDF 文件不会以语义相关的方式存储文本，而是以便于屏幕显示或打印的方式存储。这使得从 PDF 中提取文本变得困难。  

如果扫描一个文档，生成的 PDF 通常包含扫描图像。扫描仪会运行 OCR 软件，并将识别的文本置于图像的背景中。这些由扫描仪 OCR 生成的文本可以通过 pypdf 提取。然而，建议直接使用 OCR 软件，因为错误可能会累积：  

1. OCR 软件可能无法完美识别文本。  
2. 它将文本存储在不适合文本提取的格式中。  
3. pypdf 解析时可能会出错。  

因此，可以将 PDF 文件分为以下三种类型：  

1. **数字生成的 PDF 文件**  
   - 由计算机生成，可能包含图像、文本、链接、大纲（即书签）、JavaScript 等。  
   - 放大时文本依然清晰。  

2. **扫描的 PDF 文件**  
   - 包含扫描的页面，存储为 PDF 文件，实际上只是这些图像的容器。  
   - 无法复制文本，不包含链接、大纲或 JavaScript。  

3. **OCR 处理的 PDF 文件**  
   - 扫描仪运行了 OCR 软件，将识别的文本置于图像背景中。  
   - 可以复制文本，但看起来仍然是扫描件。放大时，可以看到像素化的痕迹。  

### 我们是否可以始终使用 OCR？

或许你会想，是否总是使用 OCR 软件更有意义？毕竟，如果 PDF 文件是数字生成的，可以直接将其渲染为图像。  

我并不建议这样做。  

像 pypdf 这样的文本提取软件可以利用 PDF 中比图像更多的信息，例如字体、编码、字符间的典型距离等。  

这意味着，在容易混淆的字符（如 `oO0ö`）方面，pypdf 具有明显优势。  
**pypdf 不会混淆字符**——它只读取文件中存储的内容。  

此外，在处理罕见字符（如 🤰）时，pypdf 也更具优势。OCR 软件通常无法正确识别表情符号。  

---

## 防止文本提取的尝试

如果分享 PDF 的人希望防止文本提取，有以下几种方法可以实现：  

1. 将 PDF 内容存储为图像。  
2. [使用混淆字体](https://stackoverflow.com/a/43466923/562769)。  

然而，如果人们仍然能够阅读文档，则无法完全阻止文本提取。  
在最坏的情况下，人们可以截屏、打印、扫描，然后通过 OCR 软件识别文本。  
