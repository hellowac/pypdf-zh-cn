# 文档编写

---

## API 参考

### 方法/函数的文档字符串

我们使用 Google 风格的文档字符串：

```python
def example(param1: int, param2: str) -> bool:
    """
    带有 PEP 484 类型注解的示例函数。

    Args:
      param1: 第一个参数。
      param2: 第二个参数。

    Returns:
      返回值。成功返回 True，否则返回 False。

    Raises:
      AttributeError: ``Raises`` 部分列出了所有与接口相关的异常。
      ValueError: 如果 `param2` 等于 `param1`。

    Examples:
        示例应使用 doctest 格式编写，说明如何使用此函数。

        >>> print([i for i in example_generator(4)])
        [0, 1, 2, 3]
    """
```

* 各部分的顺序为：  
  (1) **Args**（参数说明）  
  (2) **Returns**（返回值）  
  (3) **Raises**（异常）  
  (4) **Examples**（示例）

* 如果函数没有返回值，请移除 **Returns** 部分。  
* 属性（Properties）不应包含任何部分。

---

## Issues 和 PRs

* **Issue** 用于讨论“我们想要实现什么”。  
* **PR**（Pull Request）用于讨论“我们如何实现它”。

---

## 提交消息

我们希望在 `main` 分支中拥有描述清晰的提交记录。因此，每个 Pull Request（PR）都会被**压缩**（squash）。这意味着无论 PR 中包含多少次提交，最终都会合并为一个提交记录到 `main` 分支中。

* PR 的标题将作为合并提交消息的**第一行**。  
* 提交中的**第一条评论**将作为提交消息的正文。  

更多细节请参考 [开发者介绍](intro.md#commit-messages)。