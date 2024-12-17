# 提取附件

PDF 文档可以包含附件。附件有一个名称，但可能不是唯一的。因此，`reader.attachments["attachment_name"]` 的值是一个列表。

您可以通过以下方式提取所有附件：

```python
from pypdf import PdfReader

reader = PdfReader("example.pdf")

for name, content_list in reader.attachments.items():
    for i, content in enumerate(content_list):
        with open(f"{name}-{i}", "wb") as fp:
            fp.write(content)
```
