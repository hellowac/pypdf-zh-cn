### 开发者指南

pypdf 是一个库，因此它的用户是开发者。本文件并非面向用户，而是提供给希望参与 pypdf 开发的人。

---

## 安装依赖

```bash
pip install -r requirements/dev.txt
```

---

## 运行测试

请参阅 [使用 pytest 测试 pypdf](testing.md)。

---

## sample-files Git 子模块

引入 `sample-files` 子模块的原因是：我们希望保持 pypdf 仓库的体积小巧，同时也需要一个广泛的测试套件。这两个目标在某种程度上是相互矛盾的。

- `resources` 文件夹应包含一组核心示例，涵盖我们通常需要测试的大部分情况。  
- `sample-files` 文件夹可能包含更多边缘情况、更大文件尺寸下的行为，以及由不同 PDF 生成工具产生的文件。

若要获取 `sample-files` 文件夹，请执行以下命令：

```bash
git submodule update --init
```

---

## 工具：Git 和 pre-commit

**Git** 是一个用于版本控制的命令行工具。如果你不熟悉它，可以通过 [ohmygit](https://ohmygit.org/) 进行学习。

**GitHub** 是托管 pypdf 项目的服务平台。Git 是免费且开源的，而 GitHub 是微软提供的付费服务，但在许多情况下免费使用。

**[pre-commit](https://pypi.org/project/pre-commit/)** 是一个基于 git hooks 的命令行工具，用于自动执行代码检查和格式化等任务。这可以帮助你避免代码风格和质量问题。

执行以下命令以启用 pre-commit：

```bash
pre-commit install
```

完成后，每次运行 `git commit` 时，pre-commit 将自动执行。

---

## 提交消息规范

清晰的提交消息可以帮助他人快速理解提交的内容，而无需查看代码变更。提交消息的第一行用于 [自动生成 CHANGELOG](https://github.com/py-pdf/pypdf/blob/main/make_release.py)，因此格式应为：

```
PREFIX: 描述

详细内容
```

### 前缀说明

| 前缀   | 含义与描述                                                                 |
|--------|---------------------------------------------------------------------------|
| **SEC** | 安全性改进，通常用于修复可能导致无限循环的问题。                              |
| **BUG** | 修复 bug。如果有相关 issue，请在详细内容中添加 `Closes #123`，其中 123 为 GitHub issue 编号。建议编写回归测试（修复前失败，修复后通过）。用户代码的 bug 属于此类，测试代码或 CI 问题不算。 |
| **ENH** | 添加新功能！详细内容中说明功能的用途。                                       |
| **DEP** | 弃用功能。可以是标记某功能为“即将移除”或实际移除该功能。                       |
| **PI**  | 性能改进。例如生成的 PDF 文件大小的减少或处理速度的提升。                      |
| **ROB** | 稳健性改进，更好地处理损坏的 PDF 文件。                                       |
| **DOC** | 文档更新或改进。                                                           |
| **TST** | 添加或调整测试用例。                                                       |
| **DEV** | 改进开发者体验，例如调整 pre-commit 或 CI 配置。                             |
| **MAINT** | 维护工作。包括性能优化、重构代码等。                                         |
| **STY** | 代码风格改进。例如使代码风格更一致，或改善面向用户的错误信息。                 |

### 重要说明

前缀用于自动生成 CHANGELOG。每个 PR 必须有且仅有一个前缀。如果你认为多个前缀适用，请选择列表中靠前的一个。

## Pull Request 大小

**小型 Pull Request (PR)** 更受欢迎，因为它们通常更容易被合并。例如，如果你需要修复一些拼写错误、进行代码风格调整、添加新功能以及修复一个 bug，那么这可以拆分成 3 到 4 个独立的 PR。

每个 PR 必须是完整的。这意味着，如果你引入一个新功能，该功能必须在该 PR 中完成，并且包含针对该功能的测试。

---

## 基准测试

我们需要持续关注性能表现，因此设有一些基准测试。

请参阅 [py-pdf.github.io/pypdf/dev/bench](https://py-pdf.github.io/pypdf/dev/bench/) 了解更多。
