# PDF 版本支持

PDF 有以下版本：

* 1993年：1.0
* 1994年：1.1
* 1996年：1.2
* 1999年：1.3
* 2001年：1.4
* 2003年：1.5
* 2004年：1.6
* 2008年：1.7，ISO 32000-1:2008
* 2017年：2.0，ISO 32000-2:2017

PDF 的基本格式没有变化，但增加了新功能。pypdf 可能能够在不完全支持 PDF 2.0 所有功能的情况下，执行你所需的操作。

## pypdf 支持的 PDF 功能

| 功能                                       | PDF 版本     | pypdf 支持     |
| ------------------------------------------ |:------------:|:-------------:|
| CMaps                                      | 1.4          | ✅            |
| 透明图形                                   | 1.4          | ✅            |
| 内容流压缩                                 | 1.5          | ✅            |
| 交叉引用流                                 | 1.5          | ❓            |
| 对象流                                     | 1.5          | ✅            |
| 可选内容组（OCGs）                         | 1.5          | ❓            |
| AES 加密                                   | 1.6          | ✅            |

有关更多功能，请参见 [PDF 历史](https://en.wikipedia.org/wiki/History_of_PDF)。

有些 PDF 功能不被 pypdf 支持，但可以使用其他库来处理：

* [pyHanko](https://pyhanko.readthedocs.io/en/latest/index.html)：对 PDF 进行加密签名（[#302](https://github.com/py-pdf/pypdf/issues/302)）
* [camelot-py](https://pypi.org/project/camelot-py/)：表格提取（[#231](https://github.com/py-pdf/pypdf/issues/231)）