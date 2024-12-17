# 合并 PDF 文件

## 基本示例

```python
from pypdf import PdfWriter

merger = PdfWriter()

for pdf in ["file1.pdf", "file2.pdf", "file3.pdf"]:
    merger.append(pdf)

merger.write("merged-pdf.pdf")
merger.close()
```

有关更多详细信息，请参见 Paul Rooney 在 [StackOverflow](https://stackoverflow.com/questions/3444645/merge-pdf-files) 上的精彩回答。

## 显示更多合并选项

```python
from pypdf import PdfWriter

merger = PdfWriter()

input1 = open("document1.pdf", "rb")
input2 = open("document2.pdf", "rb")
input3 = open("document3.pdf", "rb")

# 将输入1文档的前3页添加到输出文档
merger.append(fileobj=input1, pages=(0, 3))

# 将输入2的第一页插入到输出文档的第二页之后
merger.merge(position=2, fileobj=input2, pages=(0, 1))

# 将整个输入3文档附加到输出文档的末尾
merger.append(input3)

# 将结果写入输出 PDF 文档
output = open("document-output.pdf", "wb")
merger.write(output)

# 关闭文件描述符
merger.close()
output.close()
```

## append

`append` 在 `PdfWriter` 中稍作扩展。有关更多详细信息，请参阅 {func}`~pypdf.PdfWriter.append`。

### 示例

```python
# 附加 source.pdf 的前10页
writer.append("source.pdf", (0, 10))

# 附加 reader 的第一页和第10页，并创建大纲
writer.append(reader, "page 1 and 10", [0, 9])
```

在合并过程中，相应的命名目标也会被导入。

如果您希望将页面插入到目标的中间位置，可以使用 `merge`（它提供了插入位置）。
如果需要，您还可以多次插入相同的页面，甚至可以使用基于列表的语法：

```python
# 插入第2页和第3页，并在其前后插入第1页
writer.append(reader, [0, 1, 0, 2, 0])
```

## add_page / insert_page

推荐使用 `append` 或 `merge` 代替。

## 合并表单

在合并表单时，一些表单字段可能具有相同的名称，这会阻止访问某些数据。

在添加源 PDF 之前，应添加一个分组字段，以防止这种情况发生。
原始字段将通过添加组名来识别。

例如，在调用 `reader.add_form_topname("form1")` 之后，之前名为 `field1` 的字段现在在调用 `reader.get_form_text_fields(True)` 或 `reader.get_fields()` 时会被识别为 `form1.field1`。

之后，您可以使用 `writer.append` 或 `writer.merge` 完整或部分地附加输入 PDF。如果您插入一组页面，则只会列出那些字段。

## reset_translation

在克隆过程中，如果一个对象已经被克隆，它将不会再次被克隆，而是返回指向该先前克隆对象的指针。因此，如果您添加/合并已经添加过的页面，第二次添加的仍然是同一个对象。如果您稍后修改这两页中的任何一页，这两页可以独立修改。

要重置，请调用 `writer.reset_translation(reader)`。

## 高级克隆

为了防止页面/对象之间的副作用，所有链接的对象在合并时都会进行克隆。

如果您使用 `PdfWriter.append/merge/add_page/insert_page`，此过程将自动应用。
如果您想在“手动”附加对象之前克隆一个对象，可以使用任何 *PdfObject* 的 `clone` 方法：

```python
cloned_object = object.clone(writer)
```

如果您尝试克隆一个已经属于 writer 的对象，它将返回相同的对象：

```python
assert cloned_object == object.clone(writer)
```

如果您尝试克隆一个对象两次，它将返回先前克隆的对象：

```python
assert object.clone(writer) == object.clone(writer)
```

请注意，如果您克隆一个对象，您也会克隆其下的所有对象，包括 *IndirectObject* 指向的对象。因此，如果您克隆一个包含一些条目的页面（例如 `"/B"`），不仅是第一个条目，所有链式条目和这些条目可以被读取的页面也会被复制。这意味着您可能会复制许多对象，这些对象将被保存在输出 PDF 中。

为了防止这种情况，您可以提供一个要忽略的字段列表：

```python
new_page = writer.add_page(reader.pages[0], excluded_fields=["/B"])
```

### 合并旋转页面

如果您正在处理旋转的页面，您可能希望在合并之前调用 {func}`~pypdf._page.PageObject.transfer_rotation_to_content` 来避免错误的旋转结果：

```python
for page in writer.pages:
    if page.rotation != 0:
        page.transfer_rotation_to_content()
    page.merge_page(background, over=False)
```
