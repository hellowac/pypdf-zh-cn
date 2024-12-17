# pypdf 如何写入 PDF 文件

pypdf 使用 {py:class}`PdfWriter <pypdf.PdfWriter>` 来写入 PDF 文件。pypdf 提供 {py:class}`PdfObject <pypdf.generic.PdfObject>` 及其多个子类，这些类实现了 {py:meth}`write_to_stream <pypdf.generic.PdfObject.write_to_stream>` 方法。{py:meth}`PdfWriter.write <pypdf.PdfWriter.write>` 方法通过调用引用对象的 `write_to_stream` 方法实现文件写入。

{py:meth}`PdfWriter.write_stream <pypdf.PdfWriter.write_stream>` 方法包括以下核心步骤：

1. **`_sweep_indirect_references`**：此步骤确保正确处理对象的循环引用。它将所有循环引用对象的引用编号添加到外部引用映射中，以便自引用页面树可以正确引用新对象位置，而不是复制页面对象的新副本。
2. **使用 `_write_pdf_structure` 写入文件头和主体**：此步骤将 PDF 头部和对象写入输出流。这包括 PDF 版本信息（例如 `%PDF-1.7`）以及组成 PDF 内容的对象，如页面、注释和表单字段。同时记录这些对象的位置（字节偏移量），以便后续生成交叉引用表（xref table）。
3. **使用 `_write_xref_table` 写入交叉引用表**：根据存储的对象位置，此步骤生成并写入交叉引用表。交叉引用表包含 PDF 文件中每个对象的字节偏移量，从而实现对对象的快速随机访问。
4. **使用 `_write_trailer` 写入文件尾部**：此步骤将文件尾部信息写入输出流。文件尾部包含重要信息，例如 PDF 中对象的数量、根对象（Catalog）的位置以及包含元数据的 Info 对象。尾部还指定交叉引用表的位置。

---

## 其他库的实现方式

分析其他软件的设计和实现方式，有助于我们改进自身的设计决策。

### fpdf2

[fpdf2](https://pypi.org/project/fpdf2/) 提供了一个 [`PDFObject` 类](https://github.com/PyFPDF/fpdf2/blob/master/fpdf/syntax.py)，其中的 `serialize` 方法大致对应于 `pypdf.PdfObject.write_to_stream`。其他相似之处包括：

- [fpdf.output.OutputProducer.buffersize](https://github.com/PyFPDF/fpdf2/blob/master/fpdf/output.py#L370-L485) 与 {py:meth}`pypdf.PdfWriter.write_stream <pypdf.PdfWriter.write_stream>`  
- [fpdf.syntax.Name](https://github.com/PyFPDF/fpdf2/blob/master/fpdf/syntax.py#L124) 与 {py:class}`pypdf.generic.NameObject <pypdf.generic.NameObject>`  
- [fpdf.syntax.build_obj_dict](https://github.com/PyFPDF/fpdf2/blob/master/fpdf/syntax.py#L222) 与 {py:class}`pypdf.generic.DictionaryObject <pypdf.generic.DictionaryObject>`  
- [fpdf.structure_tree.NumberTree](https://github.com/PyFPDF/fpdf2/blob/master/fpdf/structure_tree.py#L17) 与 {py:class}`pypdf.generic.TreeObject <pypdf.generic.TreeObject>`  

---

### pdfrw

[pdfrw](https://pypi.org/project/pdfrw/) 的设计与 pypdf 不同，它更倾向于使用标准的 Python 对象（如 `bool`、`float`、`string`），尽量避免将其包装成自定义对象。但 pdfrw 仍提供了以下类：

- [PdfArray](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/objects/pdfarray.py#L13)  
- [PdfDict](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/objects/pdfdict.py#L49)  
- [PdfName](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/objects/pdfname.py#L65)  
- [PdfString](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/objects/pdfstring.py#L322)  
- [PdfIndirect](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/objects/pdfindirect.py#L10)  

pdfrw 的核心类包括：  
- [PdfReader](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/pdfreader.py#L26)  
- [PdfWriter](https://github.com/pmaupin/pdfrw/blob/master/pdfrw/pdfwriter.py#L224)  