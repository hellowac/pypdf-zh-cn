# 与 PDF 表单的交互

## 读取表单字段

```python
from pypdf import PdfReader

reader = PdfReader("form.pdf")
fields = reader.get_form_text_fields()
fields == {"key": "value", "key2": "value2"}

# 你也可以获取所有字段：
fields = reader.get_fields()
```

## 填充表单

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("form.pdf")
writer = PdfWriter()

page = reader.pages[0]
fields = reader.get_fields()

writer.append(reader)

writer.update_page_form_field_values(
    writer.pages[0],
    {"fieldname": "some filled in text"},
    auto_regenerate=False,
)

with open("filled-out.pdf", "wb") as output_stream:
    writer.write(output_stream)
```

一般来说，你总是需要使用 `auto_regenerate=False`。该参数默认为 `True`，为了兼容旧版本，但这会标记 PDF 处理程序重新计算字段的渲染，并可能在用户打开生成的 PDF 时触发“保存更改”对话框。

## 关于表单字段和注释的一些说明

PDF 表单字段具有双重性质：

* 在根对象中，存在一个 `/AcroForm` 结构。
  其中可能包含（可选）：
  - 一些全局元素（字体、资源等）
  - 一些全局标志（例如 `/NeedAppearances`（通过 `update_page_form_field_values()` 中的 `auto_regenerate` 参数设置/清除），指示读取程序是否应在文档启动时重新渲染视觉字段）
  - `/XFA`，包含一个 XDP 格式的表单（描述由某些查看器呈现的表单的特定 XML）；`/XFA` 表单会覆盖页面内容
  - `/Fields`，包含一个间接引用数组，引用上级 _Field_ 对象（根对象）

* 在页面的 `/Annots` 中，你会发现 `/Widget` 注释，用于定义视觉呈现。

为了补充此概述：

* 字段的核心特定属性是：
  - `/FT`：字段类型（按钮、文本、选择或签名）。
  - `/T`：字段名称的一部分。
  - `/V`：字段的值，格式根据字段类型而变化。
  - `/DV`：当执行重置表单操作时，字段恢复的默认值。
* 为了简化可读性，_Field_ 对象和 _Widget_ 对象可以合并，包含所有属性。
* 字段可以进行层次组织，即一个字段可以放置在另一个字段下。在这种情况下，`/Parent` 将包含一个间接对象，提供自底向上的链接，而 `/Kids` 是一个包含间接对象的数组，用于自顶向下的导航； _Widget_ 对象仍然需要进行视觉呈现。调用它们时，使用 *完全限定的字段名称*（所有父对象的名称用 `.` 分隔）

  例如，假设有两个（视觉）字段都叫 _city_，但分别附加在 _sender_ 和 _receiver_ 下；相应的完整名称将是 _sender.city_ 和 _receiver.city_。
* 当一个字段在多个页面上重复时，字段对象将有多个 _Widget_ 对象在 `/Kids` 中。这些对象是纯粹的 _widget_，不包含任何 _field_ 特定的数据。
* 如果字段仅存储隐藏值，则不需要 _Widgets_。

在 _pypdf_ 中，字段是从 `/Fields` 数组中提取的：

```python
from pypdf import PdfReader

reader = PdfReader("form.pdf")
fields = reader.get_fields()
```

```python
from pypdf import PdfReader
from pypdf.constants import AnnotationDictionaryAttributes

reader = PdfReader("form.pdf")
fields = []
for page in reader.pages:
    for annot in page.annotations:
        annot = annot.get_object()
        if annot[AnnotationDictionaryAttributes.Subtype] == "/Widget":
            fields.append(annot)
```

然而，虽然两段代码相似，但它们之间有一些非常重要的区别。最重要的是，第一段代码会返回一个字段对象的列表，而第二段则返回更通用的类似字典的对象。两个对象列表*大多数情况下*会引用相同的底层 PDF 对象，这意味着你会发现 `obj_taken_from_first_list.indirect_reference == obj_taken_from_second_list.indirect_reference`。字段对象通常更为便捷，因为暴露的数据可以通过明确命名的属性进行访问。然而，更通用的类似字典的对象会包含字段对象没有暴露的数据，例如 Rect（widget 在页面上的位置）。因此，正确的方法取决于你的使用场景。

此外，还需要注意的是，这两个列表并不*总是*指向相同的底层 PDF 对象。例如，如果表单包含单选按钮，你会发现 `reader.get_fields()` 会获取父对象（单选按钮组），而 `page.annotations` 会返回所有子对象（单独的单选按钮）。

```{note}
请记住，字段不存储在页面中；如果你使用 `add_page()`，字段结构不会被复制。建议使用 `.append()` 并提供适当的参数。
```

在 `/Fields` 中缺少 _field_ 对象时，`writer.reattach_fields()` 将解析页面的注释并重新附加它们。此修复不能猜测中间字段，并且不会报告使用相同 _name_ 的字段。

## 确定字段使用的页面

为了方便定位页面字段，你可以使用 `PdfReader` 或 `PdfWriter` 的 `get_pages_showing_field` 方法。此方法接受一个字段对象，一个代表字段的 *PdfObject*（从 `_root_object["/AcroForm"]["/Fields"]` 提取）。该方法返回一个页面列表，因为一个字段可以有多个小部件，如前所述（例如单选按钮或显示在多个页面上的文本）。

然后，可以像往常一样通过 `page.page_number` 获取页面号。
