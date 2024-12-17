# 减小 PDF 文件大小

有多种方法可以减小给定 PDF 文件的大小。最简单的一种方法是删除内容（例如图像）或页面。

## 删除重复内容

有些 PDF 文档可能多次包含相同的对象。例如，如果一张图片在 PDF 中出现了三次，它可能被嵌入了三次。或者，它可能只嵌入一次，并被引用两次。

当向 PdfWriter 添加数据时，数据会被复制，同时保持原始格式。例如，如果两页包含相同的图像，而该图像在源文档中是重复的，则该对象将在 PdfWriter 对象中重复。

此外，当你删除文档中的对象时，pypdf 无法轻松识别这些对象是否在其他地方使用，或者用户是否希望保留它们。当写入 PDF 文件时，这些对象将隐藏在文件中（文件的一部分，但不显示）。

为了减小文件大小，可以使用压缩调用：`writer.compress_identical_objects(remove_identicals=True, remove_orphans=True)`

* `remove_identicals` 启用/禁用压缩合并相同的对象。
* `remove_orphans` 启用/禁用抑制未使用的对象。

建议在写入文件/流之前应用此过程。

这取决于 PDF 文件的具体情况，但我们已经看到在一个真实的 PDF 文件中，通过此方法文件大小减少了 86%（从 5.7 MB 降到 0.8 MB）。

## 删除图像

```python
from pypdf import PdfWriter

writer = PdfWriter(clone_from="example.pdf")

writer.remove_images()

with open("out.pdf", "wb") as f:
    writer.write(f)
```

## 降低图像质量

如果我们降低 PDF 中图像的质量，有时可以减少整个 PDF 文件的大小。这取决于降低质量的图像能压缩到什么程度。

```python
from pypdf import PdfWriter

writer = PdfWriter(clone_from="example.pdf")

for page in writer.pages:
    for img in page.images:
        img.replace(img.image, quality=80)

with open("out.pdf", "wb") as f:
    writer.write(f)
```

## 无损压缩

pypdf 支持 FlateDecode 过滤器，使用 zlib/deflate 压缩方法。它是一种无损压缩，这意味着生成的 PDF 看起来完全一样。

可以通过 `{meth}`page.compress_content_streams <pypdf._page.PageObject.compress_content_streams>` 对页面应用 Deflate 压缩：

```python
from pypdf import PdfWriter

writer = PdfWriter(clone_from="example.pdf")

for page in writer.pages:
    page.compress_content_streams()  # 这会消耗 CPU！

with open("out.pdf", "wb") as f:
    writer.write(f)
```

`page.compress_content_streams` 使用 [`zlib.compress`](https://docs.python.org/3/library/zlib.html#zlib.compress) 并支持 `level` 参数：`level=0` 表示不压缩，`level=9` 表示最高压缩。

使用此方法，我们已经看到一个实际 PDF 文件的大小减少了 70%（从 11.8 MB 降到 3.5 MB）。

## 删除源

当从页面列表中删除一页时，其内容仍然存在于 PDF 文件中。这意味着该数据可能仍然在其他地方被使用。

仅仅从页面列表中删除页面会减少页面数量，但不会减少文件大小。为了完全排除内容，应该避免通过 `PdfWriter.append()` 函数添加这些页面。相反，应该只选择需要包含的页面（注意：[PR #1843](https://github.com/py-pdf/pypdf/pull/1843) 将添加页面删除功能）。

在某些情况下，可能会遇到 PDF 格式不佳的问题，例如所有页面都链接到同一个资源。在这种情况下，删除对特定页面的引用变得没有意义，因为所有页面只有一个源。

裁剪是一个无效的减小文件大小的方法，因为它仅调整查看框，而不调整源图像的外部部分。因此，已不再显示的内容仍会存在于 PDF 文件中。

## 进一步优化

演示文稿 [Putting a Squeeze on Your PDF](https://youtube.com/watch?v=tgOABUhVwFs) 提供了其他优化建议。一个关键点是，大多数显著的大小优化通常来自图像和字体的修改。然而，字体优化（如替换、合并和子集化）目前不在 pypdf 的功能范围内。