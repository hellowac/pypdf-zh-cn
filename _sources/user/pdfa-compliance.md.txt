# PDF/A 合规性

PDF/A 是一种专门化的、符合 ISO 标准的 PDF 版本，专为电子文档的长期保存和归档设计。它通过将所有必要的字体、图像和元数据嵌入文档本身，确保文件保持可访问、可阅读，并忠实于原始外观。通过遵守严格的指南并尽量减少对外部资源或专有软件的依赖，PDF/A 确保内容的一致性和可靠复制，防止其因技术变更和过时而无法访问。

## PDF/A 版本

* **PDF/A-1**：基于 PDF 1.4，PDF/A-1 是该标准的第一个版本，分为两个级别：PDF/A-1a（级别 A，确保可访问性）和 PDF/A-1b（级别 B，确保视觉保真）。
    * **级别 B**（基本）：确保视觉保真和基本的归档要求。
    * **级别 A**（可访问）：包括级别 B 的所有内容，但增加了可访问性要求，如标记、Unicode 字符映射和逻辑结构。
* **PDF/A-2**：基于 PDF 1.7（ISO 32000-1），PDF/A-2 在 PDF/A-1 的基础上增加了功能和改进，同时保持与 PDF/A-1b（级别 B）文档的兼容性。
    * **级别 B**（基本）：类似于 PDF/A-1b，但支持 PDF 1.7 的特性，如透明图层。
    * **级别 U**（Unicode）：确保 Unicode 映射，但没有 PDF/A-1a（级别 A）所要求的完整可访问性要求。
    * **级别 A**（可访问）：类似于 PDF/A-1a。
* **PDF/A-3**：基于 PDF 1.7（ISO 32000-1），PDF/A-3 与 PDF/A-2 类似，但允许将非 PDF/A 文件作为附件嵌入，从而实现将源数据或补充数据与 PDF/A 文档一起归档。这对发票等场景非常有用，可以附加 XML 文件。
* **PDF/A-4**：基于 PDF 2.0（ISO 32000-2），PDF/A-4 引入了新的功能和改进，增强了归档和可访问性。以前的级别被 PDF/A-4f（确保视觉保真并允许附件）和 PDF/A-4e（工程，允许 3D 内容）替代。

## PDF/A-1b

与其他 PDF 文档不同，PDF/A-1b 文档必须满足以下要求：

* **MarkInfo 对象**：MarkInfo 对象是 PDF/A 文件中的一个字典对象，提供有关文档逻辑结构和标记的信息。MarkInfo 对象指示文档是否已标记，是否包含可选内容，或是否具有描述逻辑排列的结构树，如标题、段落、列表和表格。通过包含 MarkInfo 对象，PDF/A 确保电子文档对残障用户可访问，例如使用屏幕阅读器或其他辅助技术的用户。
* **嵌入字体**：文档中使用的所有字体必须嵌入，以确保在不同设备和系统上的一致文本呈现。
* **色彩空间**：DeviceRGB 是一个设备依赖的色彩空间，依赖于输出设备的特定特性，这可能导致在不同设备上色彩渲染不一致。为了实现准确和一致的色彩表示，PDF/A 要求使用设备独立的色彩空间，如基于 ICC 的色彩配置文件。
* **XMP（可扩展元数据平台）元数据**：XMP 元数据提供了一种标准化和可扩展的方式来存储有关文档及其属性的必要信息。XMP 元数据是一个基于 XML 的格式，嵌入在 PDF/A 文件中。它包含多种类型的信息，如文档标题、作者、创建和修改日期、关键字和版权信息，以及 PDF/A 特有的细节，如符合性级别和输出意图。

## 验证

[VeraPDF](https://docs.verapdf.org/install/) 是 PDF/A 的常用验证工具。

有几个在线验证工具可以让你轻松上传文档进行验证：

* [pdfen.com](https://www.pdfen.com/pdf-a-validator)
* [avepdf.com](https://avepdf.com/pdfa-validation)：提供错误报告
* [pdfa.org](https://pdfa.org/pdfa-online-verification-service/)
* [visual-paradigm.com](https://online.visual-paradigm.com/de/online-pdf-editor/pdfa-validator/) - 可以将 PDF 转换为 PDF/A
* [pdf2go.com](https://www.pdf2go.com/validate-pdfa)
* [slub-dresden.de](https://www.slub-dresden.de/veroeffentlichen/dissertationen-habilitationen/elektronische-veroeffentlichung/slub-pdfa-validator) 提供了指向规范相关部分的链接。

## pypdf 和 PDF/A

目前，pypdf 并未对 PDF/A 提供任何保证。
[非常欢迎支持](https://github.com/py-pdf/pypdf/labels/is-pdf%2Fa-compliance)。