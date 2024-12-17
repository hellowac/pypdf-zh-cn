# 添加查看器首选项

可以设置 PDF 文件的查看器首选项。
详见 [PDF 1.7 规范](https://opensource.adobe.com/dc-acrobat-sdk-docs/pdfstandards/PDF32000_2008.pdf) 的 §12.2。

请注意，`/ViewerPreferences` 字典在默认情况下并不存在。
如果它尚未存在，必须通过调用 `{func}`~pypdf.PdfWriter.create_viewer_preferences` 方法创建。

如果在使用 `{class}`~pypdf.PdfReader` 读取的 PDF 文件中存在查看器首选项，可以通过 `{attr}`~pypdf.PdfReader.viewer_preferences` 属性访问它们。
否则，`{attr}`~pypdf.PdfReader.viewer_preferences` 属性将被设置为 `None`。

## 示例

```python
from pypdf import PdfWriter
from pypdf.generic import ArrayObject, NumberObject

writer = PdfWriter()

writer.create_viewer_preferences()

# /HideToolbar
writer.viewer_preferences.hide_toolbar = True
# /HideMenubar
writer.viewer_preferences.hide_menubar = True
# /HideWindowUI
writer.viewer_preferences.hide_windowui = True
# /FitWindow
writer.viewer_preferences.fit_window = True
# /CenterWindow
writer.viewer_preferences.center_window = True
# /DisplayDocTitle
writer.viewer_preferences.display_doctitle = True

# /NonFullScreenPageMode
writer.viewer_preferences.non_fullscreen_pagemode = "/UseNone"  # 默认
writer.viewer_preferences.non_fullscreen_pagemode = "/UseOutlines"
writer.viewer_preferences.non_fullscreen_pagemode = "/UseThumbs"
writer.viewer_preferences.non_fullscreen_pagemode = "/UseOC"

# /Direction
writer.viewer_preferences.direction = "/L2R"  # 默认
writer.viewer_preferences.direction = "/R2L"

# /ViewArea
writer.viewer_preferences.view_area = "/CropBox"
# /ViewClip
writer.viewer_preferences.view_clip = "/CropBox"
# /PrintArea
writer.viewer_preferences.print_area = "/CropBox"
# /PrintClip
writer.viewer_preferences.print_clip = "/CropBox"

# /PrintScaling
writer.viewer_preferences.print_scaling = "/None"
writer.viewer_preferences.print_scaling = "/AppDefault"  # 根据 PDF 规范的默认设置

# /Duplex
writer.viewer_preferences.duplex = "/Simplex"
writer.viewer_preferences.duplex = "/DuplexFlipShortEdge"
writer.viewer_preferences.duplex = "/DuplexFlipLongEdge"

# /PickTrayByPDFSize
writer.viewer_preferences.pick_tray_by_pdfsize = True
# /PrintPageRange
writer.viewer_preferences.print_pagerange = ArrayObject(
    [NumberObject("1"), NumberObject("10"), NumberObject("20"), NumberObject("30")]
)
# /NumCopies
writer.viewer_preferences.num_copies = 2

for i in range(40):
    writer.add_blank_page(10, 10)

with open("output.pdf", "wb") as output_stream:
    writer.write(output_stream)
```

以斜杠字符开头的名称是 PDF 文件格式的一部分。
在这里列出这些名称是为了方便从 PDF 规范中查找 pypdf 文档中的相关内容。