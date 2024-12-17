# 读取 PDF 注释

PDF 2.0 定义了以下几种注释类型：

* Text
* Link
* FreeText
* Line
* Square
* Circle
* Polygon
* PolyLine
* Highlight
* Underline
* Squiggly
* StrikeOut
* Caret
* Stamp
* Ink
* Popup
* FileAttachment
* Sound
* Movie
* Screen
* Widget
* PrinterMark
* TrapNet
* Watermark
* 3D
* Redact
* Projection
* RichMedia

通常，可以通过以下方式读取注释：

```python
from pypdf import PdfReader

reader = PdfReader("annotated.pdf")

for page in reader.pages:
    if "/Annots" in page:
        for annotation in page["/Annots"]:
            obj = annotation.get_object()
            print({"subtype": obj["/Subtype"], "location": obj["/Rect"]})
```

以下是读取三种常见注释的示例：

## 文本(Text)

```python
from pypdf import PdfReader

reader = PdfReader("example.pdf")

for page in reader.pages:
    if "/Annots" in page:
        for annotation in page["/Annots"]:
            subtype = annotation.get_object()["/Subtype"]
            if subtype == "/Text":
                print(annotation.get_object()["/Contents"])
```

## 高亮(Highlights)

```python
from pypdf import PdfReader

reader = PdfReader("example.pdf")

for page in reader.pages:
    if "/Annots" in page:
        for annotation in page["/Annots"]:
            subtype = annotation.get_object()["/Subtype"]
            if subtype == "/Highlight":
                coords = annotation.get_object()["/QuadPoints"]
                x1, y1, x2, y2, x3, y3, x4, y4 = coords
```

## 附件(Attachments)

```python
from pypdf import PdfReader

reader = PdfReader("example.pdf")

attachments = {}
for page in reader.pages:
    if "/Annots" in page:
        for annotation in page["/Annots"]:
            subtype = annotation.get_object()["/Subtype"]
            if subtype == "/FileAttachment":
                fileobj = annotation.get_object()["/FS"]
                attachments[fileobj["/F"]] = fileobj["/EF"]["/F"].get_data()
```