# 向 PDF 添加印章或水印

添加印章或水印是两种常见的 PDF 文件操作方式。印章是将某些内容添加到文档的顶部，而水印则位于文档的背景中。

## 印章（叠加）/ 水印（底层）

印章和水印的过程是相同的，唯一的区别是你需要将 `over` 参数设置为 `True` 来添加印章，设置为 `False` 来添加水印。

如果不需要对印章进行转换，你可以使用 `{func}`~pypdf._page.PageObject.merge_page`：

```python
from pypdf import PdfReader, PdfWriter

stamp = PdfReader("bg.pdf").pages[0]
writer = PdfWriter(clone_from="source.pdf")
for page in writer.pages:
    page.merge_page(stamp, over=False)  # 此处设置为 False 以添加水印

writer.write("out.pdf")
```

否则，如果你需要在合并印章到内容页面之前进行平移、旋转、缩放等操作，使用 `{func}`~pypdf._page.PageObject.merge_transformed_page` 和 `{class}`~pypdf.Transformation`：

```python
from pathlib import Path
from typing import List, Union

from pypdf import PdfReader, PdfWriter, Transformation


def stamp(
    content_pdf: Union[Path, str],
    stamp_pdf: Union[Path, str],
    pdf_result: Union[Path, str],
    page_indices: Union[None, List[int]] = None,
):
    stamp_page = PdfReader(stamp_pdf).pages[0]

    writer = PdfWriter()
    # page_indices 可以是页面的列表（数组），元组用于定义范围
    reader = PdfReader(content_pdf)
    writer.append(reader, pages=page_indices)

    for content_page in writer.pages:
        content_page.merge_transformed_page(
            stamp_page,
            Transformation().scale(0.5),
        )

    writer.write(pdf_result)


stamp("example.pdf", "stamp.pdf", "out.pdf")
```

如果你遇到水印/印章旋转错误的问题，可以尝试在相应的页面上使用 `{func}`~pypdf._page.PageObject.transfer_rotation_to_content`，以修复页面框架。

印章示例：
![stamp.png](stamp.png)

水印示例：
![watermark.png](watermark.png)

## 直接印章图像

上述代码仅适用于已经是 PDF 格式的印章。如果你想使用图像作为印章，可以通过 [Pillow](https://pypi.org/project/Pillow/) 将图像轻松转换为 PDF 格式的图像。

```python
from io import BytesIO
from pathlib import Path
from typing import List, Union

from PIL import Image
from pypdf import PageRange, PdfReader, PdfWriter, Transformation


def image_to_pdf(stamp_img: Union[Path, str]) -> PdfReader:
    img = Image.open(stamp_img)
    img_as_pdf = BytesIO()
    img.save(img_as_pdf, "pdf")
    return PdfReader(img_as_pdf)


def stamp_img(
    content_pdf: Union[Path, str],
    stamp_img: Union[Path, str],
    pdf_result: Union[Path, str],
    page_indices: Union[PageRange, List[int], None] = None,
):
    # 将图像转换为 PDF
    stamp_pdf = image_to_pdf(stamp_img)

    # 然后使用上面相同的印章代码
    stamp_page = stamp_pdf.pages[0]

    writer = PdfWriter()

    reader = PdfReader(content_pdf)
    writer.append(reader, pages=page_indices)
    for content_page in writer.pages:
        content_page.merge_transformed_page(
            stamp_page,
            Transformation(),
        )

    with open(pdf_result, "wb") as fp:
        writer.write(fp)


stamp_img("example.pdf", "example.png", "out.pdf")
```