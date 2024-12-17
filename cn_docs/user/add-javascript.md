# 向 PDF 添加 JavaScript

不同的 PDF 阅读器对 JavaScript 的支持程度不同，有些甚至完全不支持。

Adobe 提供了其支持的文档，详情请见：
[https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/index.html](https://opensource.adobe.com/dc-acrobat-sdk-docs/library/jsapiref/index.html)

## 打开时启动打印窗口

```python
from pypdf import PdfWriter

writer = PdfWriter(clone_from="example.pdf")

# 添加 JavaScript，使得打开该 PDF 时启动打印窗口。
writer.add_js("this.print({bUI:true,bSilent:false,bShrinkToFit:true});")

# 写入 pypdf-output.pdf。
with open("pypdf-output.pdf", "wb") as fp:
    writer.write(fp)
```