# pypdf 历史

## 起源：pyPdf (2005-2010)

在 2005 年，[Mathieu Fenniak] 推出了 pyPdf，作为一个“PDF 工具包...”，专注于以下功能：

- 文档操作：按页拆分、合并和拼接；
- 文档内部分析；
- 页面裁剪；
- 文档加密和解密。

PyPI 上的最后一个版本是 [pyPdf 1.13](https://pypi.org/project/pyPdf/#history)，发布于 2010 年。

## PyPDF2 的诞生 (2011-2016)

2011 年底，在与 Mathieu 和其他人讨论后，Phaseit 将 PyPDF2 作为 pyPdf 的一个分支发布到 GitHub。最初的推动力是处理更广泛的 PDF 输入实例；Phaseit 的商业工作经常遇到一些“野外”中的 PDF 实例，需要管理（主要是合并和分页），但这些实例偏离 PDF 标准太多，pyPdf 无法读取它们。PyPDF2 能够读取更多种类的实际 PDF 实例。

无论是 pyPdf 还是 PyPDF2，都并不旨在成为“通用”的工具，即提供所有可能的 PDF 相关功能。值得注意的是，Mariano Reingart 的 [pyfpdf] 与 [ReportLab] 更为相似，因为 ReportLab 和 pyfpdf 都强调文档生成。有趣的是，pyfpdf 内置了一个基本的 HTML→PDF 转换器，而 PyPDF2 并不支持 HTML。

那么，PyPDF2 的真正用途是什么呢？可以考虑一下流行的 [pdftk]。PyPDF2 完成了 pdftk 的工作，而且是在当前的 Python 进程中完成的，并且它处理的 PDF 格式变体范围更广。PyPDF2 有自己的常见问题解答，回答了其他一些出现的问题。

2012 年 3 月，Reddit [/r/python 社区] 简短而间接地讨论了 PyPDF2。

核心开发者/维护者是 Matthew Stamy。

## PyPDF3 和 PyPDF4 (2018 - 2022)

有两种方式试图让 PyPDF2 重新活跃起来：PyPDF3 和 PyPDF4。

PyPDF3 在 2018 年发布了第一个版本，并在 2022 年 2 月发布了最后一个版本。它并未从 PyPDF2 获得大量用户基础。

PyPDF4 只有一个 2018 年的发布版本。

## PyPDF2：重生 (2022)

Martin Thoma 于 2022 年 4 月接管了 PyPDF2 的维护工作。当时有超过 100 个待处理的 PR 和 321 个待解决的问题。

[pubpub-zz](https://github.com/pubpub-zz) 非常活跃，特别是在文本提取方面。

[Matthew Peveler](https://github.com/MasterOdin) 在代码审查和项目决策方面提供了大量帮助。

[exiledkingcc](https://github.com/exiledkingcc) 为现代加密方案添加了支持。

## pypdf：回归本源 (2023 至今)

为了让初学者更容易上手，PyPDF2 被重新合并回 pypdf。现在是全小写，没有版本号。我们希望开发 PyPDF3 和 PyPDF4 的开发者也能加入我们。

与 `PyPDF2 >= 3.0.0` 相比，`pypdf >= 3.1.0` 现在提供了以下功能：

* 支持 AES 读写。不仅支持 PyCryptoDome，还支持 cryptography。
* 改进了文本提取，例如数学内容的提取。[pypdf 现在可以与 Tika、pypdfium2 和 PyMuPDF 相媲美](https://github.com/py-pdf/benchmarks)。
* 支持注释
* 性能改进和 bug 修复
* 支持页面标签

  [Mathieu Fenniak]: https://mathieu.fenniak.net/
  [pyfpdf]: https://github.com/reingart/pyfpdf
  [ReportLab]: https://www.reportlab.com/software/opensource/rl-toolkit/
  [pdftk]: https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/
  [/r/python 社区讨论]: https://www.reddit.com/r/Python/comments/qsvfm/pypdf2_updates_pypdf_pypdf2_is_an_opensource/