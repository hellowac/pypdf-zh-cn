# PDF 格式

建议查阅 **PDF 规范** 来获取详细信息和进一步澄清。

* [PDF 规范档案](https://pdfa.org/resource/pdf-specification-archive/)
* [便携式文档格式参考手册，1993. ISBN 0-201-62628-4](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/pdfreference1.0.pdf)
* [ISO 32000-1:2008 (PDF 1.7)](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/PDF32000_2008.pdf)
* ISO 32000-2:2020 (PDF 2.0)

以下内容仅为该格式的**大致概述**。

---

## 整体结构

一个 PDF 文件包括：

1. **头部 (Header)**：包含 PDF 的版本，例如 `%PDF-1.7`
2. **主体 (Body)**：包含一系列间接对象 (indirect objects)
3. **交叉引用表 (xref)**：包含主体中间接对象的列表
4. **尾部 (Trailer)**

---

## 交叉引用表 (xref)

交叉引用表是主体中间接对象的一个索引表，它通过指向文件中的具体位置，实现对对象的快速访问。

其结构如下：

```text
xref 42 5
0000001000 65535 f
0000001234 00000 n
0000001987 00000 n
0000011987 00000 n
0000031987 00000 n
```

逐行解析：

* `xref`：标识交叉引用表的开始。
* `42`：当前 xref 部分中第一个对象的编号；`5` 表示 xref 表中条目的总数。
* 每个对象条目格式为 `nnnnnnnnnn ggggg n`：
   - `nnnnnnnnnn`：对象的字节偏移量，指出对象在文件中的位置。
   - `ggggg`：对象的生成号，表示对象的版本历史。
   - `n` 表示该对象为**正常使用对象**，`f` 表示该对象为**空闲对象**。
     - 第一个空闲对象的生成号始终为 `65535`，它是所有空闲对象链表的头部。
     - 正常对象的生成号始终为 `0`。生成号允许 PDF 文件包含同一对象的多个版本，用于版本控制机制。

---

## 主体 (Body)

主体由一系列**间接对象**组成，格式如下：

```
counter generation_number << the_object >> endobj
```

* `counter` (整数)：对象的唯一标识符。
* `generation_number` (整数)：对象的生成号。
* `the_object`：对象本身，可以为空。以 `/Keyword` 开头，指定对象类型。
* `endobj`：标识对象的结束。

**示例**（摘自 `test_reader.py::test_get_images_raw`）：

```text
1 0 obj << /Count 1 /Kids [4 0 R] /Type /Pages >> endobj
2 0 obj << >> endobj
3 0 obj << >> endobj
4 0 obj << /Contents 3 0 R /CropBox [0.0 0.0 2550.0 3508.0]
 /MediaBox [0.0 0.0 2550.0 3508.0] /Parent 1 0 R
 /Resources << /Font << >> >>
 /Rotate 0 /Type /Page >> endobj
5 0 obj << /Pages 1 0 R /Type /Catalog >> endobj
```

---

## 尾部 (Trailer)

尾部的格式如下：

```text
trailer << /Root 5 0 R
           /Size 6
        >>
startxref 1234
%%EOF
```

逐行解析：

* `trailer <<`：标识**尾部字典**的开始，以 `>>` 结束。
* `startxref`：一个关键字，后面跟随 `xref` 关键字的字节位置。由于尾部始终位于文件末尾，这使得读取器可以快速定位到 xref 表。
* `%%EOF`：文件结束标识。

**尾部字典**是一个键值对列表。键的定义见 PDF 参考规范 1.7 的表 15，例如 `/Root` 和 `/Size`（这两个键是必须的）：

* `/Root`（字典）：包含文档目录。
   - `5`：目录字典的对象编号。
   - `0`：目录字典的生成号。
   - `R`：表示该对象是对目录字典的引用。
* `/Size`（整数）：表示 xref 表中的条目总数。

---

## 读取 PDF 文件

大多数 PDF 文件是经过压缩的。如果要读取它们，首先需要解压：

```bash
pdftk crazyones.pdf output crazyones-uncomp.pdf uncompress
```

然后将 `crazyones-uncomp.pdf` 重命名为 `crazyones-uncomp.txt`，并在你喜欢的 IDE 或文本编辑器中打开它。