# 测试

`pypdf` 使用 [`pytest`](https://docs.pytest.org/en/7.1.x/) 进行测试。

要运行测试，您需要通过运行 `pip install -r requirements/ci.txt` 或如果使用 Python ≥ 3.11，则运行 `pip install -r requirements/ci-3.11.txt` 来安装 CI（持续集成）要求。

## 取消选择一组测试

`pypdf` 使用以下 `pytest` 标记：

* `slow`：需要超过 5 秒的测试。
* `samples`：需要初始化 [`sample-files` git 子模块](https://github.com/py-pdf/sample-files) 的测试。截至 2022 年 10 月，这大约为 25 MB。
* `enable_socket`：下载 PDF 文档的测试。这些文档会被存储在本地，因此只需要下载一次。截至 2022 年 10 月，这大约为 200 MB。
  * 为了成功运行测试，请提前下载大多数文档：`python -c "from tests import download_test_pdfs; download_test_pdfs()"`

您可以通过 `pytest -m "not enable_socket"` 或 `pytest -m "not samples"` 来禁用它们。
您甚至可以禁用所有这些：`pytest -m "not enable_socket" -m "not samples" -m "not slow"`。

请注意，这会减少测试覆盖率。CI 将始终测试所有文件。

## 单元测试中的文档字符串

单元测试中的文档字符串的第一行应以一种方式编写，使其可以加上 "This test ensures that ..."，例如：

* 错误的 XML 在 xmp_metadata 中被优雅地处理。
* 身份验证返回其输入。
* 正确提取 xmp_modify_date。

这样，像 [`pytest-testdox`](https://pypi.org/project/pytest-testdox/) 这样的插件可以在测试运行时生成非常漂亮的输出。这类似于 [mocha.js](https://mochajs.org/) 的输出。

如果测试是回归测试，请写：

> 该测试是针对问题 #1234 的回归测试。

如果回归测试只是其他测试的一个参数，请将其作为该参数的注释添加。

## 评估进行中的拉取请求版本

您可能想测试一个尚未发布的拉取请求版本。最简单的方法是使用 pip 从 git 安装该版本：

a) 转到拉取请求并确定仓库和分支。

例如：仓库：__pubpub-zz__ / 分支：__iss2200__：
![PR Header example](PR_Header_example.png)

b) 然后，您可以使用 pip 从 git 安装该版本：

示例：
```
pip install git+https://github.com/pubpub-zz/pypdf.git@iss2200
```