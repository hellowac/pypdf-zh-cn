# pypdf 的范围

pypdf 应该具备哪些功能，哪些功能将永远不具备？

pypdf 旨在简化与 PDF 文档的交互。pypdf 可以执行的核心任务包括：

* 文档操作：拆分、合并、裁剪和转换 PDF 文件的页面
* 数据提取：从 PDF 文档中提取文本和元数据
* 安全性：解密/加密 PDF 文档

以下是一些指示，表明某个功能应该由 pypdf 来实现：

* 任务需要深入了解 PDF 格式
* 当前需要大量代码，或者甚至无法用 pypdf 完成
* 该功能既不属于“应由用户代码实现”也不属于“超出范围”
* 它已经出现在带有 [is-feature 标签](https://github.com/py-pdf/pypdf/labels/is-feature) 的问题列表中。

[月球任务扩展](https://github.com/py-pdf/pypdf/discussions/1181)是我们希望拥有的功能，但目前无法添加（欢迎提交 PR 😉）

## 应由用户代码实现

以下是一些指示，表明某个功能应该放到用户代码中实现（而不是 pypdf 中）：

1. 使用场景非常具体，大多数人不会遇到相同的需求。
2. 它可以在不需要了解 PDF 规范的情况下完成。
3. 它无法在没有（非 PDF）领域知识的情况下完成。任何与你所在行业相关的内容。

## 超出范围

虽然这个列表是无限长的，但有一些话题被多次询问过。

这些话题超出了 pypdf 的范围，永远不会成为 pypdf 的一部分：

1. **光学字符识别 (OCR)**：OCR 是从图像中提取文本。这与 pypdf 所做的文本提取完全不同。请注意，图像可以包含在 PDF 文档中。在扫描文档的情况下，整个页面就是图像。有些扫描仪会自动执行 OCR 并在扫描页面后面添加文本层。pypdf 可以利用这个文本层（如果它存在的话）。一个值得注意的开源 OCR 项目是 [tesseract](https://github.com/tesseract-ocr/tesseract)。
2. **格式转换**：将 docx / HTML 转换为 PDF，或将 PDF 转换为这些格式。你可以查看 [`pdfkit`](https://pypi.org/project/pdfkit/) 和类似的项目。

目前超出范围，但如果有足够的贡献者，可能会添加：

* **数字签名支持** ([参考票据](https://github.com/py-pdf/pypdf/issues/302))：加密技术复杂，必须做到正确。pypdf 目前没有足够的活跃贡献者来正确添加数字签名支持。目前，[pyhanko](https://pypi.org/project/pyHanko/) 似乎是最佳选择。
* **从头开始生成 PDF**：pypdf 可以操作现有的 PDF 文档，添加注释，合并/拆分/裁剪/转换页面。它可以添加空白页面。但如果你想生成发票，你可能需要查看 [`reportlab`](https://pypi.org/project/reportlab/) / [`fpdf2`](https://pypi.org/project/fpdf2/) 或类似的文档转换工具，如 [`pdfkit`](https://pypi.org/project/pdfkit/)。
* **在 PDF 中替换单词**：[从 PDF 中提取文本很困难](../user/extract-text.md#why-text-extraction-is-hard)。以可靠的方式替换文本更难。例如，一个单词可能被拆分成多个令牌。因此，在某些情况下，这并不是简单的“查找和替换”。
* **（不）提取页眉/页脚/页码**：虽然可以应用启发式方法，但无法始终确保成功。PDF 文档根本不包含页眉/页脚/页码的相关信息。

### 库与应用程序

还值得指出的是，`pypdf` 设计为一个库，而不是应用程序。这有几个含义：

* 执行：pypdf 不能直接执行，而是只能在 pypdf 用户编写的程序中调用。相比之下，应用程序是由它自己执行的。
* 依赖性：pypdf 应该有一个最小的依赖集，并且仅在严格必要时才限制依赖性。相比之下，应用程序应该安装在与其他应用程序隔离的环境中，并可以锁定其依赖项。

如果你正在寻找一种通过 Shell 与 PDF 文件交互的方式，你应该编写一个使用 pypdf 的脚本，或者使用 [`pdfly`](https://pypi.org/project/pdfly/)。