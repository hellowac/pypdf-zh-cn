# pypdf 与 X 的对比

pypdf 是一个 [免费的] 开源纯 Python PDF 库，能够分割、合并、裁剪和转换 PDF 文件的页面。它还可以向 PDF 文件中添加自定义数据、查看选项和密码。pypdf 还可以从 PDF 文件中提取文本和元数据。

## PyMuPDF 和 PikePDF

[PyMuPDF] 是一个连接 [MuPDF] 的 Python 绑定，[PikePDF] 是一个连接 [QPDF] 的 Python 绑定。

虽然这两个库在各种用例中表现优秀，但即使它们支持某个用例，使用它们也不总是可行的。这两个库都依赖于 C 库，这使得安装变得更加复杂，并可能引发安全问题。对于 MuPDF，你可能还需要购买商业许可证。

pypdf 的一个核心特性是它是纯 Python 编写的。这意味着没有 C 的依赖。它已经被使用超过 10 年，因此有大量通过 StackOverflow 和互联网上的示例提供的支持。

## pypdf

PyPDF2 已经合并回 `pypdf`。开发继续在 `pypdf` 上进行。

## PyPDF3 和 PyPDF4

开发和维护开源软件是非常耗时的，尤其是在 pypdf 这种完全没有报酬的情况下。持续的支持非常困难。

pypdf 最初于 2012 年在 PyPI 上发布，并持续发布更新直到 2016 年。从 2016 年到 2022 年没有更新——但人们仍在使用它。

作为一款免费软件，曾有人尝试分叉并继续开发它。PyPDF3 首次发布于 2018 年，至今仍在更新。PyPDF4 只有 2018 年的一次发布。

我（Martin Thoma，pypdf 和 PyPDF2 的当前维护者）希望我们能够将社区重新聚焦于一个开发路径。我已经弃用了 PyPDF2，转而支持 pypdf，现在 pypdf 拥有比 PyPDF2 更多的功能和更清晰的接口。请参见 [pypdf 的历史](history.md)。

  [free]: https://en.wikipedia.org/wiki/Free_software
  [PyMuPDF]: https://pypi.org/project/PyMuPDF/
  [MuPDF]: https://mupdf.com/
  [PikePDF]: https://pypi.org/project/pikepdf/
  [QPDF]: https://github.com/qpdf/qpdf

## pdfminer.six 和 pdfplumber

[`pdfminer.six`](https://pypi.org/project/pdfminer.six/) 能够提取 [字体大小](https://stackoverflow.com/a/69962459/562769) / 字体粗细（加粗）。它没有写入 PDF 文件的能力。

[`pdfplumber`](https://pypi.org/project/pdfplumber/) 是一个专注于从 PDF 文档中提取数据的库。由于 `pdfplumber` 是基于 `pdfminer.six` 构建的，因此它 **没有导出或修改 PDF 文件的能力**（参见 [#440 (讨论)](https://github.com/jsvine/pdfplumber/discussions/440#discussioncomment-803880)）。然而，`pdfplumber` 能够将 PDF 文件转换为图像，[在图像上绘制线条和矩形](https://github.com/jsvine/pdfplumber#drawing-methods)，并将其保存为图像文件。请注意，图像转换是通过 ImageMagick 完成的（参见 [`pdfplumber` 的文档](https://github.com/jsvine/pdfplumber#visual-debugging)）。

`pdfplumber` 社区在回答问题方面非常活跃，且该库在 2023 年 5 月仍在维护中。

## pdfrw / pdfrw2

我没有使用这些库的经验。如果你了解 pypdf 和 [`pdfrw`](https://pypi.org/project/pdfrw/)，请添加一个对比！

请注意，还有 [`pdfminer`](https://pypi.org/project/pdfminer/) 库，它已经不再维护。另外，还有 [`pdfrw2`](https://pypi.org/project/pdfrw2/)，但它没有强大的社区支持。

## 文档生成

有一些（Python）[工具用于生成 PDF 文档](https://github.com/py-pdf/awesome-pdf#generators)，而 pypdf 不是其中之一。

## CLI 应用程序

pypdf 是一个纯 Python 的 PDF 库。如果你正在寻找一个可以从终端使用的应用程序，可以试试 [`pdfly`](https://pdfly.readthedocs.io/en/latest/)。