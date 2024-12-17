# 使用 pypdf 流式传输数据

在某些情况下，你可能希望避免显式地将内容保存为磁盘上的文件，例如，当你希望将 PDF 存储在数据库或 AWS S3 中时。

pypdf 支持将数据流式传输到类文件对象：

```python
from io import BytesIO

# 准备示例
with open("example.pdf", "rb") as fh:
    bytes_stream = BytesIO(fh.read())

# 从 bytes_stream 中读取
reader = PdfReader(bytes_stream)

# 写入 bytes_stream
writer = PdfWriter()
with BytesIO() as bytes_stream:
    writer.write(bytes_stream)
```

## 将 PDF 直接写入 AWS S3

假设你想操作一个 PDF 并将其直接写入 AWS S3，而不需要先将文档写入文件。我们将原始 PDF 存储在 `raw_bytes_data` 中作为 `bytes`，并希望设置密码为 `my-secret-password`：

```python
from io import BytesIO

import boto3
from pypdf import PdfReader, PdfWriter


reader = PdfReader(BytesIO(raw_bytes_data))
writer = PdfWriter()

# 将所有页面添加到写入器
for page in reader.pages:
    writer.add_page(page)

# 给新的 PDF 添加密码
writer.encrypt("my-secret-password")

# 将新的 PDF 保存到文件
with BytesIO() as bytes_stream:
    writer.write(bytes_stream)
    bytes_stream.seek(0)
    s3 = boto3.client("s3")
    s3.write_get_object_response(
        Body=bytes_stream, RequestRoute=request_route, RequestToken=request_token
    )
```

## 从云服务直接读取 PDF

一种选择是先下载文件，然后将本地文件路径传递给 `PdfReader`。另一种选择是获取字节流。

对于 AWS S3，可以像这样操作：

```python
from io import BytesIO

import boto3
from pypdf import PdfReader


s3 = boto3.client("s3")
obj = s3.get_object(Body=csv_buffer.getvalue(), Bucket="my-bucket", Key="my/doc.pdf")
reader = PdfReader(BytesIO(obj["Body"].read()))
```

如果是使用 Google Cloud 存储，可以这样做：

```python
from io import BytesIO

from google.cloud import storage

# 必须设置 os.environ["GOOGLE_APPLICATION_CREDENTIALS"]
storage_client = storage.Client()
blob = storage_client.bucket("my-bucket").blob("mydoc.pdf")
file_stream = BytesIO()
blob.download_to_file(file_stream)
reader = PdfReader(file_stream)
```