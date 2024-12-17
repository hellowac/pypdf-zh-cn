.. pypdf documentation main file, created by
   sphinx-quickstart on Thu Apr  7 20:13:19 2022.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

欢迎使用 pypdf  
=================  

pypdf 是一个 `免费 <https://en.wikipedia.org/wiki/Free_software>`_ 且开源的纯 Python PDF 库，能够对 PDF 文件的页面进行拆分、合并、裁剪和转换操作。它还支持向 PDF 文件添加自定义数据、查看选项以及密码保护功能。此外, pypdf 还可以从 PDF 中提取文本和元数据。

有关使用 pypdf 操作 PDF 的命令行工具，请参阅 `pdfly <https://github.com/py-pdf/pdfly>`_ 。

您可以通过 GitHub 贡献代码到 `pypdf <https://github.com/py-pdf/pypdf>`_ 。  

.. toctree::
   :caption: 用户指南
   :maxdepth: 1

   user/installation
   user/migration-1-to-2
   user/robustness
   user/suppress-warnings
   user/metadata
   user/extract-text
   user/post-processing-in-text-extraction
   user/extract-images
   user/extract-attachments
   user/encryption-decryption
   user/merging-pdfs
   user/cropping-and-transforming
   user/reading-pdf-annotations
   user/adding-pdf-annotations
   user/add-watermark
   user/add-javascript
   user/viewer-preferences
   user/forms
   user/streaming-data
   user/file-size
   user/pdf-version-support
   user/pdfa-compliance


.. toctree::
   :caption: API 参考
   :maxdepth: 1

   modules/PdfReader
   modules/PdfWriter
   modules/Destination
   modules/DocumentInformation
   modules/Field
   modules/Fit
   modules/PageObject
   modules/PageRange
   modules/PaperSize
   modules/RectangleObject
   modules/Transformation
   modules/XmpInformation
   modules/annotations
   modules/constants
   modules/errors
   modules/generic
   modules/PdfDocCommon

.. toctree::
   :caption: 开发者指南
   :maxdepth: 1

   dev/intro
   dev/pdf-format
   dev/pypdf-parsing
   dev/pypdf-writing
   dev/cmaps
   dev/deprecations
   dev/documentation
   dev/testing
   dev/releasing

.. toctree::
   :caption: 关于 pypdf
   :maxdepth: 1

   meta/CHANGELOG
   meta/changelog-v1
   meta/project-governance
   meta/taking-ownership
   meta/history
   meta/CONTRIBUTORS
   meta/scope-of-pypdf
   meta/comparisons
   meta/faq

索引表
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
