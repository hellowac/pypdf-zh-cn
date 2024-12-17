# 添加 PDF 注释

## 附件

```python
from pypdf import PdfWriter

writer = PdfWriter()
writer.add_blank_page(width=200, height=200)

data = b"any bytes - typically read from a file"
writer.add_attachment("smile.png", data)

with open("output.pdf", "wb") as output_stream:
    writer.write(output_stream)
```

## 自由文本

如果你想在一个框中添加文本，如下所示：

![](free-text-annotation.png)

你可以使用 `{class}`~pypdf.annotations.FreeText` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import FreeText

# 填充写入器与所需的页面
pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 创建注释并添加
annotation = FreeText(
    text="Hello World\nThis is the second line!",
    rect=(50, 550, 200, 650),
    font="Arial",
    bold=True,
    italic=True,
    font_size="20pt",
    font_color="00ff00",
    border_color="0000ff",
    background_color="cdcdcd",
)

# 设置注释标志为4，用于可打印注释。
# 有关其他选项（例如隐藏等），请参阅“AnnotationFlag”。
annotation.flags = 4

writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 文本

文本注释看起来像这样：

![](text-annotation.png)

## 线条

如果你想添加一条线，如下所示：

![](annotation-line.png)

你可以使用 `{class}`~pypdf.annotations.Line` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Line

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加线条
annotation = Line(
    text="Hello World\nLine2",
    rect=(50, 550, 200, 650),
    p1=(50, 550),
    p2=(200, 650),
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 多边形线

如果你想添加一条如下所示的多边形线：

![](annotation-polyline.png)

你可以使用 `{class}`~pypdf.annotations.PolyLine` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import PolyLine

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加多边形线
annotation = PolyLine(
    vertices=[(50, 550), (200, 650), (70, 750), (50, 700)],
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 矩形

如果你想添加一个矩形，如下所示：

![](annotation-square.png)

你可以使用 `{class}`~pypdf.annotations.Rectangle` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Rectangle

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加矩形
annotation = Rectangle(
    rect=(50, 550, 200, 650),
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

如果你希望矩形填充颜色，可以使用 `interiour_color="ff0000"` 参数。

此方法使用 PDF 格式中的“方形”注释类型。

## 椭圆

如果你想添加一个圆形，如下所示：

![](annotation-circle.png)

你可以使用 `{class}`~pypdf.annotations.Ellipse` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Ellipse

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加椭圆
annotation = Ellipse(
    rect=(50, 550, 200, 650),
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 多边形

如果你想添加一个多边形，如下所示：

![](annotation-polygon.png)

你可以使用 `{class}`~pypdf.annotations.Polygon` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Polygon

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加多边形
annotation = Polygon(
    vertices=[(50, 550), (200, 650), (70, 750), (50, 700)],
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 弹出窗口

管理标记的弹出窗口，外观如下：

![](annotation-popup.png)

你可以使用 `{py:class}`~pypdf.annotations.Popup` 类：

```python
from pypdf.annotations import Popup, Text

# 设置
writer = pypdf.PdfWriter()
writer.append(os.path.join(RESOURCE_ROOT, "crazyones.pdf"), [0])

# 操作
text_annotation = writer.add_annotation(
    0,
    Text(
        text="Hello World\nThis is the second line!",
        rect=(50, 550, 200, 650),
        open=True,
    ),
)

popup_annotation = Popup(
    rect=(50, 550, 200, 650),
    open=True,
    parent=text_annotation,  # 使用add_annotation的输出结果
)

writer.write("annotated-pdf-popup.pdf")
```

你必须使用 `add_annotation()` 返回的结果，因为这是此弹出窗口注释应该关联的父注释。

## 链接

如果你想添加一个链接，可以使用 `{class}`~pypdf.annotations.Link` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Link

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加链接
annotation = Link(
    rect=(50, 550, 200, 650),
    url="https://martin-thoma.com/",
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

你还可以添加内部链接：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Link
from pypdf.generic import Fit

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

# 添加链接
annotation = Link(
    rect=(50, 550, 200, 650),
    target_page_index=3,
    fit=Fit(fit_type="/FitH", fit_args=(123,)),
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```

## 文本标记注释

文本标记注释是指文档中特定的一段文字。

这些注释稍微复杂一些，因为你需要确切知道文本所在的位置，即所谓的“Quad点”。

### 高亮显示

如果你想像这样高亮显示文本：

![](annotation-highlight.png)

你可以使用 `{class}`~pypdf.annotations.Highlight` 类：

```python
from pypdf import PdfReader, PdfWriter
from pypdf.annotations import Highlight
from pypdf.generic import ArrayObject, FloatObject

pdf_path = os.path.join(RESOURCE_ROOT, "crazyones.pdf")
reader = PdfReader(pdf_path)
page = reader.pages[0]
writer = PdfWriter()
writer.add_page(page)

rect = (50, 550, 200, 650)
quad_points = [rect[0], rect[1], rect[2], rect[1], rect[0], rect[3], rect[2], rect[3]]

# 添加高亮注释
annotation = Highlight(
    rect=rect,
    quad_points=ArrayObject([FloatObject(quad_point) for quad_point in quad_points]),
)
writer.add_annotation(page_number=0, annotation=annotation)

# 将带有注释的文件写入磁盘
with open("annotated-pdf.pdf", "wb") as fp:
    writer.write(fp)
```
