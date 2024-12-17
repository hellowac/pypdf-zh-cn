# 提取图像

```{note}
为了使用以下代码，您需要安装可选的依赖项，请参见[安装指南](installation.md)。
```

每一页的 PDF 文档都可以包含任意数量的图像。图像文件的名称可能不是唯一的。

```python
from pypdf import PdfReader

reader = PdfReader("example.pdf")

page = reader.pages[0]

for count, image_file_object in enumerate(page.images):
    with open(str(count) + image_file_object.name, "wb") as fp:
        fp.write(image_file_object.data)
```

# 其他图像

一些其他对象也可以包含图像，例如印章注释。

例如，文档中包含如下印章：
[test_stamp.pdf](https://github.com/user-attachments/files/15751424/test_stamp.pdf)

您可以使用以下代码从注释中提取图像：

```python
from pypdf import PdfReader

reader = PdfReader("test_stamp.pdf")
im = (
    reader.pages[0]["/Annots"][0]
    .get_object()["/AP"]["/N"]["/Resources"]["/XObject"]["/Im4"]
    .decode_as_image()
)

im.show()
```