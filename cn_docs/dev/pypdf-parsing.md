# pypdf 如何解析 PDF 文件

pypdf 使用 {class}`~pypdf.PdfReader` 来解析 PDF 文件。方法 {py:meth}`PdfReader.read <pypdf.PdfReader.read>` 展示了解析的基本结构：

1. **查找并读取交叉引用表 (xref) 和尾部 (trailer)**：  
   交叉引用表 (xref table) 是一个包含字节偏移量的表，用于指示文件中对象的位置。尾部 (trailer) 提供了附加信息，例如根对象 (Catalog) 和包含元数据的 Info 对象。

2. **解析对象**：  
   在定位到 xref 表和尾部之后，pypdf 继续解析 PDF 中的对象。PDF 中的对象可以是多种类型，例如字典 (dictionaries)、数组 (arrays)、流 (streams) 和简单数据类型（如整数、字符串）。pypdf 解析这些对象并将其存储在 {py:meth}`PdfReader.resolved_objects <pypdf.PdfReader.resolved_objects>` 中，并通过 {py:meth}`cache_indirect_object <pypdf.PdfReader.cache_indirect_object>` 进行填充。

3. **解码内容流**：  
   PDF 的内容通常存储在**内容流**中，这些流是 PDF 操作符和操作数的序列。pypdf 通过应用流字典中指定的过滤器（例如 `FlateDecode`、`LZWDecode`）来解码这些内容流。仅当对象通过 {py:meth}`PdfReader.get_object <pypdf.PdfReader.get_object>` 请求时，才会进行此解码，该方法使用 `PdfReader._get_object_from_stream` 方法。

---

## 参考资料

[PDF 1.7 规范](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/PDF32000_2008.pdf)：  
* 7.5 文件结构  
* 7.5.4 交叉引用表  
* 7.8 内容流和资源  