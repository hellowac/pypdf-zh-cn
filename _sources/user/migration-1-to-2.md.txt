# 迁移指南：从 1.x 到 2.x  

`PyPDF2<2.0.0`（[文档](https://pypdf2.readthedocs.io/en/1.27.12/meta/history.html)）与 `PyPDF2>=2.0.0`（[文档](../meta/history.md)）差异较大。  

幸运的是，大多数更改只是简单的命名调整。本指南将帮助您从 `PyPDF2 1.x`（甚至原始的 PyPdf）迁移到 `PyPDF2>=2.0.0`。  

您可以使用更新版本运行代码，并通过运行以下命令显示弃用警告：  

```bash  
python -W all your_code.py  
```  

# 导入和模块  

- `PyPDF2.utils` 已被移除。  
- `PyPDF2.pdf` 已被移除。您可以直接从 `PyPDF2` 或 `PyPDF2.generic` 中导入所需内容。  

# 命名调整  

## 类名  

基础类已重命名，因为它们不仅可以操作文件，还可以操作 ByteIO 流。同时，`strict` 参数的默认值从 `strict=True` 改为 `strict=False`。  

- `PdfFileReader` ➔ `PdfReader`  
- `PdfFileWriter` ➔ `PdfWriter`  
- `PdfFileMerger` ➔ `PdfMerger`  

`PdfFileReader` 和 `PdfFileMerger` 不再支持 `overwriteWarnings` 参数。新行为默认 `overwriteWarnings=False`。  

## 函数、方法和属性名称  

在 `PyPDF2.xmp.XmpInformation`:

* `rdfRoot` ➔ `rdf_root`
* `xmp_createDate` ➔ `xmp_create_date`
* `xmp_creatorTool` ➔ `xmp_creator_tool`
* `xmp_metadataDate` ➔ `xmp_metadata_date`
* `xmp_modifyDate` ➔ `xmp_modify_date`
* `xmpMetadata` ➔ `xmp_metadata`
* `xmpmm_documentId` ➔ `xmpmm_document_id`
* `xmpmm_instanceId` ➔ `xmpmm_instance_id`

在 `PyPDF2.generic`:

* `readObject` ➔ `read_object`
* `convertToInt` ➔ `convert_to_int`
* `DocumentInformation.getText` ➔ `DocumentInformation._get_text` : 此方法通常不应使用；如果您需要，请告诉我。
* `readHexStringFromStream` ➔ `read_hex_string_from_stream`
* `initializeFromDictionary` ➔ `initialize_from_dictionary`
* `createStringObject` ➔ `create_string_object`
* `TreeObject.hasChildren` ➔ `TreeObject.has_children`
* `TreeObject.emptyTree` ➔ `TreeObject.empty_tree`

在许多地方:
  - `getObject` ➔ `get_object`
  - `writeToStream` ➔ `write_to_stream`
  - `readFromStream` ➔ `read_from_stream`


### PdfReader class:
  - `reader.getPage(pageNumber)` ➔ `reader.pages[page_number]`
  - `reader.getNumPages()` / `reader.numPages` ➔ `len(reader.pages)`
  - `getDocumentInfo` ➔ `metadata`
  - `flattenedPages` attribute ➔ `flattened_pages`
  - `resolvedObjects` attribute ➔ `resolved_objects`
  - `xrefIndex` attribute ➔ `xref_index`
  - `getNamedDestinations` / `namedDestinations` attribute ➔ `named_destinations`
  - `getPageLayout` / `pageLayout` ➔ `page_layout` attribute
  - `getPageMode` / `pageMode` ➔ `page_mode` attribute
  - `getIsEncrypted` / `isEncrypted` ➔ `is_encrypted` attribute
  - `getOutlines` ➔ `get_outlines`
  - `readObjectHeader` ➔ `read_object_header`
  - `cacheGetIndirectObject` ➔ `cache_get_indirect_object`
  - `cacheIndirectObject` ➔ `cache_indirect_object`
  - `getDestinationPageNumber` ➔ `get_destination_page_number`
  - `readNextEndLine` ➔ `read_next_end_line`
  - `_zeroXref` ➔ `_zero_xref`
  - `_authenticateUserPassword` ➔ `_authenticate_user_password`
  - `_pageId2Num` attribute ➔ `_page_id2num`
  - `_buildDestination` ➔ `_build_destination`
  - `_buildOutline` ➔ `_build_outline`
  - `_getPageNumberByIndirect(indirectRef)` ➔ `_get_page_number_by_indirect(indirect_ref)`
  - `_getObjectFromStream` ➔ `_get_object_from_stream`
  - `_decryptObject` ➔ `_decrypt_object`
  - `_flatten(..., indirectRef)` ➔ `_flatten(..., indirect_ref)`
  - `_buildField` ➔ `_build_field`
  - `_checkKids` ➔ `_check_kids`
  - `_writeField` ➔ `_write_field`
  - `_write_field(..., fieldAttributes)` ➔ `_write_field(..., field_attributes)`
  - `_read_xref_subsections(..., getEntry, ...)` ➔ `_read_xref_subsections(..., get_entry, ...)`

### PdfWriter class:
  - `writer.getPage(pageNumber)` ➔ `writer.pages[page_number]`
  - `writer.getNumPages()` ➔ `len(writer.pages)`
  - `addMetadata` ➔ `add_metadata`
  - `addPage` ➔ `add_page`
  - `addBlankPage` ➔ `add_blank_page`
  - `addAttachment(fname, fdata)` ➔ `add_attachment(filename, data)`
  - `insertPage` ➔ `insert_page`
  - `insertBlankPage` ➔ `insert_blank_page`
  - `appendPagesFromReader` ➔ `append_pages_from_reader`
  - `updatePageFormFieldValues` ➔ `update_page_form_field_values`
  - `cloneReaderDocumentRoot` ➔ `clone_reader_document_root`
  - `cloneDocumentFromReader` ➔ `clone_document_from_reader`
  - `getReference` ➔ `get_reference`
  - `getOutlineRoot` ➔ `get_outline_root`
  - `getNamedDestRoot` ➔ `get_named_dest_root`
  - `addBookmarkDestination` ➔ `add_bookmark_destination`
  - `addBookmarkDict` ➔ `add_bookmark_dict`
  - `addBookmark` ➔ `add_bookmark`
  - `addNamedDestinationObject` ➔ `add_named_destination_object`
  - `addNamedDestination` ➔ `add_named_destination`
  - `removeLinks` ➔ `remove_links`
  - `removeImages(ignoreByteStringObject)` ➔ `remove_images(ignore_byte_string_object)`
  - `removeText(ignoreByteStringObject)` ➔ `remove_text(ignore_byte_string_object)`
  - `addURI` ➔ `add_uri`
  - `addLink` ➔ `add_link`
  - `getPage(pageNumber)` ➔ `get_page(page_number)`
  - `getPageLayout / setPageLayout / pageLayout` ➔ `page_layout attribute`
  - `getPageMode / setPageMode / pageMode` ➔ `page_mode attribute`
  - `_addObject` ➔ `_add_object`
  - `_addPage` ➔ `_add_page`
  - `_sweepIndirectReferences` ➔ `_sweep_indirect_references`

### PdfMerger class
  - `__init__` parameter: `strict=True` ➔ `strict=False` (the `PdfFileMerger` still has the old default)
  - `addMetadata` ➔ `add_metadata`
  - `addNamedDestination` ➔ `add_named_destination`
  - `setPageLayout` ➔ `set_page_layout`
  - `setPageMode` ➔ `set_page_mode`

### Page class:
  - `artBox` / `bleedBox` / `cropBox` / `mediaBox` / `trimBox` ➔ `artbox` / `bleedbox` / `cropbox` / `mediabox` / `trimbox`
    - `getWidth`, `getHeight ` ➔ `width` / `height`
    - `getLowerLeft_x` / `getUpperLeft_x` ➔ `left`
    - `getUpperRight_x` / `getLowerRight_x` ➔ `right`
    - `getLowerLeft_y` / `getLowerRight_y` ➔ `bottom`
    - `getUpperRight_y` / `getUpperLeft_y` ➔ `top`
    - `getLowerLeft` / `setLowerLeft` ➔ `lower_left` property
    - `upperRight` ➔ `upper_right`
  - `mergePage` ➔ `merge_page`
  - `rotateClockwise` / `rotateCounterClockwise` ➔ `rotate_clockwise`
  - `_mergeResources` ➔ `_merge_resources`
  - `_contentStreamRename` ➔ `_content_stream_rename`
  - `_pushPopGS` ➔ `_push_pop_gs`
  - `_addTransformationMatrix` ➔ `_add_transformation_matrix`
  - `_mergePage` ➔ `_merge_page`

### XmpInformation class:
  - `getElement(..., aboutUri, ...)` ➔ `get_element(..., about_uri, ...)`
  - `getNodesInNamespace(..., aboutUri, ...)` ➔ `get_nodes_in_namespace(..., aboutUri, ...)`
  - `_getText` ➔ `_get_text`

### utils.py:

  - `matrixMultiply` ➔ `matrix_multiply
  - `RC4_encrypt` is moved to the security module

## 参数名称

* `PdfWriter.get_page`: `pageNumber` ➔ `page_number`
* `PyPDF2.filters` (all classes): `decodeParms` ➔ `decode_parms`
* `PyPDF2.filters` (all classes): `decodeStreamData` ➔ `decode_stream_data`
* `pagenum` ➔ `page_number`
* `PdfMerger.merge`: `position` ➔ `page_number`
* `PdfWriter.add_outline_item_destination`: `dest` ➔ `page_destination`
* `PdfWriter.add_named_destination_object`: `dest` ➔ `page_destination`
* `PdfWriter.encrypt`: `user_pwd` ➔ `user_password`
* `PdfWriter.encrypt`: `owner_pwd` ➔ `owner_password`

## 弃用

一些类/函数已被弃用且没有替换：

* `PyPDF2.utils.ConvertFunctionsToVirtualList`
* `PyPDF2.utils.formatWarning`
* `PyPDF2.isInt(obj)`: 使用 `instance(obj, int)` 替代
* `PyPDF2.u_(s)`: 直接使用 `s` 
* `PyPDF2.chr_(c)`: 使用 `chr(c)` 替代
* `PyPDF2.barray(b)`: 使用 `bytearray(b)` 替代
* `PyPDF2.isBytes(b)`: 使用 `instance(b, type(bytes()))` 替代
* `PyPDF2.xrange_fn`: 使用 `range` 替代
* `PyPDF2.string_type`: 使用 `str` 替代
* `PyPDF2.isString(s)`: 使用 `instance(s, str)` 替代
* `PyPDF2._basestring`: 使用 `str` instead
* `b_(...)` 已被删除。您通常应该能够直接使用字节对象，否则您可以[复制此内容](https://github.com/py-pdf/PyPDF2/pull/986#issuecomment-1230698069)
