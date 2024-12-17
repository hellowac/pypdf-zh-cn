# 常见问题解答

## pypdf 与 PyPDF2 有什么关系？

PyPDF2 是从原始的 pyPdf 分支出来的。在几年后，这个分支被重新合并到 `pypdf` 中（现在是全小写）。

## 支持哪些 Python 版本？

pypdf 3.0+ 支持 Python 3.6 及更高版本。
PyPDF2 2.0+ 支持 Python 3.6 及更高版本。
PyPDF2 1.27.10 支持 Python 2.7 至 3.10。

  [Matthew]: https://github.com/mstamy2
  [source]: https://github.com/py-pdf/PyPDF2/commit/24b270d876518d15773224b5d0d6c2206db29f64#commitcomment-5038317
  [this sort of thing]: https://github.com/py-pdf/PyPDF2/issues/24
  [GitHub issue]: https://github.com/py-pdf/PyPDF2/issues

## 谁在使用 pypdf？

pyPdf 被 [多个项目](https://github.com/Buyanbat/XacCRM/tree/ee78e8df967182f661b6494a86444501e7d89c8f/report/pyPdf) [嵌入](https://github.com/MyBook/calibre/tree/ca1efe3c21f6553e096dab745b3cdeb36244a5a9/src/pyPdf) [到其他项目中](https://github.com/Giacomo-De-Florio-Dev/Make_Your_PDF_Safe/tree/ec439f92243d12d54ae024668792470c6b40ee96/MakeYourPDFsafe_V1.3/PyPDF2)。这意味着 pyPdf 的代码被复制到了这些项目中。

依赖 pypdf 的项目包括：

* [Camelot](https://github.com/camelot-dev/camelot): 一个用于从 PDF 中提取表格数据的 Python 库
* [edi](https://github.com/OCA/edi): 电子数据交换模块
* [amazon-textract-textractor](https://github.com/aws-samples/amazon-textract-textractor/blob/42444b08c672607eadbdcd64f3c5adb2d85383de/helper/setup.py): 使用 Amazon Textract 分析文档，并生成多种格式的输出。
* [maigret](https://github.com/soxoj/maigret): 从成千上万的网站收集一个人的资料
* [deda](https://github.com/dfd-tud/deda): 跟踪点提取、解码和匿名化工具包
* [opencanary](https://github.com/thinkst/opencanary)
* 文档转换
  * [rst2pdf](https://github.com/rst2pdf/rst2pdf)
  * [xhtml2pdf](https://github.com/xhtml2pdf/xhtml2pdf)
  * [doc2text](https://github.com/jlsutherland/doc2text)
* [pdfalyzer](https://pypi.org/project/pdfalyzer/): 一款 PDF 分析工具，用于以极大且多彩的图表可视化 PDF 内部树状数据结构，并扫描 PDF 中嵌入的二进制流，寻找隐藏的潜在恶意内容。

## 如何引用 pypdf？

BibTeX 格式：

```
@misc{pypdf,
 title         = {The {pypdf} library},
 author        = {Mathieu Fenniak and
                  Matthew Stamy and
                  pubpub-zz and
                  Martin Thoma and
                  Matthew Peveler and
                  exiledkingcc and {pypdf Contributors}},
 year          = {2024},
 url           = {https://pypi.org/project/pypdf/}
 note          = {See https://pypdf.readthedocs.io/en/latest/meta/CONTRIBUTORS.html for all contributors}
}
```

## pypdf 使用哪种许可证？

`pypdf` 使用 [BSD-3-Clause 许可证](https://en.wikipedia.org/wiki/BSD_licenses#3-clause)，请参见 LICENSE 文件。