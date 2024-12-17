# PDF 的加密与解密

PDF 加密使用 [`RC4`](https://en.wikipedia.org/wiki/RC4) 和 [`AES`](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 算法，具有不同的密钥长度。`pypdf` 支持所有这些算法，直到 `PDF-2.0`，这是最新的 PDF 标准。

`pypdf` 使用额外的依赖来执行 `AES` 算法的加密或解密。
我们推荐使用 [`pyca/cryptography`](https://cryptography.io/en/latest/)。另外，您也可以使用 [`pycryptodome`](https://pypi.org/project/pycryptodome/)。

```{note}
请参阅 [安装指南](installation.md) 中的说明，了解如何安装与 AES 加密 PDF 交互所需的额外依赖。
```

## 加密

您可以使用密码加密 PDF：

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("example.pdf")
writer = PdfWriter(clone_from=reader)

# 为新 PDF 添加密码
writer.encrypt("my-secret-password", algorithm="AES-256")

# 将新 PDF 保存到文件
with open("encrypted-pdf.pdf", "wb") as f:
    writer.write(f)
```

算法可以是 `RC4-40`、`RC4-128`、`AES-128`、`AES-256-R5`、`AES-256` 之一。我们建议使用 `AES-256-R5`。

```{warning}
如果省略 "algorithm" 参数，`pypdf` 默认使用 `RC4`，以保持兼容性。
由于 `RC4` 不安全，您应使用 `AES` 算法。
```

## 解密

您可以使用适当的密码解密 PDF：

```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("encrypted-pdf.pdf")

if reader.is_encrypted:
    reader.decrypt("my-secret-password")

writer = PdfWriter(clone_from=reader)

# 将新 PDF 保存到文件
with open("decrypted-pdf.pdf", "wb") as f:
    writer.write(f)
```
