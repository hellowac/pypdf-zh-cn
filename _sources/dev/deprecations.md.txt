# 弃用流程

pypdf 致力于为现有用户和新用户提供卓越的库。我们在引入可能导致不兼容更改时非常谨慎，但如果这些更改从长远来看对社区有益，我们会推进实施。

我们希望并认为弃用不会经常发生。如果确实发生，用户可以依赖以下流程。

---

## 语义化版本控制

pypdf 遵循[语义化版本控制](https://semver.org/)。如果您想避免破坏性更改，请使用依赖版本固定（也称为版本锁定）。在 Python 中，可以通过在 `requirements.txt` 文件中指定所需的确切版本来实现。一个可以支持此操作的工具是 [`pip-tools`](https://pypi.org/project/pip-tools/) 提供的 `pip-compile`。

如果您使用 [Poetry](https://pypi.org/project/poetry/)，则可以通过 `poetry.lock` 文件来实现版本锁定。

---

## pypdf 如何弃用功能

假设当前的 pypdf 版本为 `x.y.z`。经过讨论（例如通过 GitHub issue），我们决定移除某个类/函数/方法。以下是具体流程：

1. **`x.y.(z+1)`**：  
   添加一个 **DeprecationWarning**（弃用警告）。如果存在替代方案，同时引入替代方案，并在警告中说明变更内容及实施时间。  
   - 文档中会告知用户该功能已弃用、实施时间及新功能。  
   - CHANGELOG 中会记录此信息。

2. **`(x+1).0.0`**：  
   删除/修改代码，进行破坏性更改，将 **DeprecationWarning** 替换为 **DeprecationError**（弃用错误）。  
   - 这样做是为了帮助之前没有注意警告的用户。  
   - CHANGELOG 中会记录此信息。

3. **`(x+2).0.0`**：  
   移除 **DeprecationError**。

---

这意味着用户将收到三次提醒：  
1. 在 CHANGELOG 中的 3 次警告；  
2. 在下一个主要版本发布前的 **DeprecationWarning**；  
3. 在下一个主要版本后的 **DeprecationError**。

---

**注意**：添加警告可能对某些用户（尤其是在 CI 环境中）造成破坏性影响。因此，必须提供充分的文档说明。