# 接管 pypdf

pypdf 目前由我，Martin Thoma 维护。我希望避免 pypdf 再次无人维护。如果我因健康问题等原因无法继续工作，这份文档将作为指南，帮助避免 pypdf 再度无人维护。

目前这只是一个抽象的场景。我很好，预计还会继续维护几年，但我已经见证过因为维护者变得不活跃而导致项目停滞多年。

我还遵循了 [GitHub 已故用户政策](https://docs.github.com/en/site-policy/other-site-policies/github-deceased-user-policy)，并添加了 [预指定继任者](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-access-to-your-personal-repositories/maintaining-ownership-continuity-of-your-personal-accounts-repositories)。

## pypdf 属于什么？

维护 pypdf 所需的资源包括：

* PyPI：[pypdf](https://pypi.org/project/pypdf/) 和 [PyPDF2](https://pypi.org/project/PyPDF2/)
* GitHub：[pypdf](https://github.com/py-pdf/pypdf)（仓库，而不是组织）
* ReadTheDocs：[pypdf](https://readthedocs.org/projects/pypdf/) 和 [PyPDF2](https://readthedocs.org/projects/pypdf2/)

## 何时可以接管？

**180 天内无活动**：如果我没有回复电子邮件（info@martin-thoma.de），并且半年内没有任何提交/合并，您可以认为 pypdf “无人维护”。

## 谁可以接管？

最好是 GitHub `py-pdf` 组织的所有者之一来接管。

从我当前的视角（Martin Thoma，2023 年 8 月 27 日），以下人员可能是候选人：

* [Lucas-C](https://github.com/Lucas-C)：他维护 fpdf2 并且是 py-pdf 的所有者
* [pubpub-zz](https://github.com/pubpub-zz)：他是 pypdf 最活跃的贡献者
* [Matthew Peveler](https://github.com/MasterOdin)：不太活跃，但他非常谨慎地处理破坏性更改，并且是经验丰富的软件开发者。
* [exiledkingcc](https://github.com/exiledkingcc)：他贡献了与加密相关的核心更改。

## 如何接管？

* PyPI：遵循 [PEP 541 – 包名保留](https://peps.python.org/pep-0541/)
* GitHub：与其他 py-pdf 组织的所有者沟通
* ReadTheDocs：遵循 [废弃项目政策](https://docs.readthedocs.io/en/latest/abandoned-projects.html)