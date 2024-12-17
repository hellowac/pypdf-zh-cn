PaperSize 类
-------------------

.. autoclass:: pypdf.PaperSize
    :members:
    :undoc-members:
    :show-inheritance:

使用 PaperSize 添加空白页
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: python
    :linenos:

    from pypdf import PaperSize, PdfWriter

    writer = PdfWriter(clone_from="sample.pdf")
    writer.add_blank_page(PaperSize.A8.width, PaperSize.A8.height)
    with open("output.pdf", "wb") as output_stream:
        writer.write(output_stream)

插入具有 PaperSize 的空白页
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: python
    :linenos:

    from pypdf import PaperSize, PdfWriter

    writer = PdfWriter(clone_from="sample.pdf")
    writer.insert_blank_page(PaperSize.A8.width, PaperSize.A8.height, 1)
    with open("output.pdf", "wb") as output_stream:
        writer.write(output_stream)
