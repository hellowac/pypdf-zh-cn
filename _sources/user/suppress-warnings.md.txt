# 异常、警告和日志消息  

pypdf 使用以下三种机制来表明出现问题的情况：  

- **异常（Exceptions）**  
  异常用于需要 pypdf 用户明确处理的错误情况。在 `strict=True` 模式下，大多数日志中处于警告级别的消息会变为异常。这在某些需要用户修复损坏 PDF 文件的应用程序中非常有用。  

- **警告（Warnings）**  
  警告是可以避免的问题，例如使用了已弃用的类、函数或参数。另一个例子是 pypdf 的某些功能缺失。在这些情况下，pypdf 用户应调整代码。警告是通过 `warnings` 模块发出的，与日志级别的 "warning" 不同。  

- **日志消息（Log messages）**  
  日志消息提供信息，用于事后分析。大多数情况下，用户可以忽略这些消息。日志消息有不同的**级别**，例如 info / warning / error，表示严重性。示例包括：pypdf 能处理的非标准 PDF 文件，或某些导致部分文本无法提取的功能缺失。  

## 异常（Exceptions）  

如果您想处理异常，需要捕获它们。例如，您可能希望从 PDF 中读取文本，作为搜索功能的一部分。  

大多数 PDF 文件不完全符合规范。在这种情况下，pypdf 需要推测文件创建时可能出现的错误类型。相关问题请参见[稳健性页面](robustness.md)。  

作为用户，您可能不在意文件是否完全符合规范，只关心是否可以读取文本。在这种情况下，可以将 pdfminer.six 作为后备方案：  

```python  
from pypdf import PdfReader  
from pdfminer.high_level import extract_text as fallback_text_extraction  

text = ""  
try:  
    reader = PdfReader("example.pdf")  
    for page in reader.pages:  
        text += page.extract_text()  
except Exception as exc:  
    text = fallback_text_extraction("example.pdf")  
```  

如果希望更具体地捕获异常，可以捕获 [`pypdf.errors.PyPdfError`](https://github.com/py-pdf/pypdf/blob/main/pypdf/errors.py)。  

## 警告（Warnings）  

[`warnings` 模块](https://docs.python.org/3/library/warnings.html)允许您忽略警告：  

```python  
import warnings  

warnings.filterwarnings("ignore")  
```  

在许多情况下，建议使用 `-W` 标志启动 Python，以查看所有警告。这在持续集成（CI）环境中尤为重要。  

## 日志消息（Log Messages）  

在某些情况下，日志消息可能过多。pypdf 提供了合理的日志消息级别，但您可以调整显示的消息类型：  

```python  
import logging  

logger = logging.getLogger("pypdf")  
logger.setLevel(logging.ERROR)  
```  

[`logging` 模块](https://docs.python.org/3/library/logging.html#logging-levels)定义了六个日志级别：  

- CRITICAL  
- ERROR  
- WARNING  
- INFO  
- DEBUG  
- NOTSET  