# 稳健性与 `strict=False`  

PDF 文件有[多个版本规范](https://pdfa.org/resource/pdf-specification-archive/)。  
PDF 2.0 的规范文档有 1003 页，这样的篇幅使得完全遵循规范变得困难。因此，许多 PDF 文件并不严格遵守规范。  

如果 PDF 文件不符合规范，往往无法确定其预期的效果。例如以下错误的 Python 代码：  

```python  
# 错误代码
function (foo, bar):  

# 可能的意图：
def function(foo, bar):  
    ...  

# 也可能是：
function = (foo, bar)  
```  

在编写解析器时，可以选择两种策略：一种是尽量宽容，尝试推测用户的意图；另一种是严格遵循规范，告诉用户他们的文件需要修复。  

pypdf 为您提供了是否严格的选择。  

pypdf 的两个核心对象为：  

- {class}`~pypdf.PdfReader`  
- {class}`~pypdf.PdfWriter`  

只有 `PdfReader` 有 `strict` 参数，因为假定您不希望生成一个不合规的 PDF 文件。  

- 如果选择 `strict=True`，pypdf 会在 PDF 文件不符合规范时抛出异常。  
- 如果选择 `strict=False`，pypdf 会尝试宽容处理，并尽可能完成合理的操作，但会记录一条警告信息。这是一种尽力而为的方式。  