# 元数据（Metadata）  

## 读取元数据  

```python  
from pypdf import PdfReader  

reader = PdfReader("example.pdf")  

meta = reader.metadata  

# 以下所有字段都可能为 None！  
print(meta.title)  
print(meta.author)  
print(meta.subject)  
print(meta.creator)  
print(meta.producer)  
print(meta.creation_date)  
print(meta.modification_date)  
```  

## 写入元数据  

```python  
from datetime import datetime  
from pypdf import PdfReader, PdfWriter  

reader = PdfReader("example.pdf")  
writer = PdfWriter()  

# 将所有页面添加到 writer  
for page in reader.pages:  
    writer.add_page(page)  

# 如果需要保留原有元数据，添加以下两行代码  
if reader.metadata is not None:  
    writer.add_metadata(reader.metadata)  

# 格式化当前日期和时间用于元数据  
utc_time = "-05'00'"  # 可选的 UTC 时间偏移  
time = datetime.now().strftime(f"D\072%Y%m%d%H%M%S{utc_time}")  

# 添加新的元数据  
writer.add_metadata(  
    {  
        "/Author": "Martin",  
        "/Producer": "Libre Writer",  
        "/Title": "Title",  
        "/Subject": "Subject",  
        "/Keywords": "Keywords",  
        "/CreationDate": time,  
        "/ModDate": time,  
        "/Creator": "Creator",  
        "/CustomField": "CustomField",  
    }  
)  

# 将新 PDF 保存到文件  
with open("meta-pdf.pdf", "wb") as f:  
    writer.write(f)  
```  

## 更新元数据  

```python  
from pypdf import PdfWriter  

writer = PdfWriter(clone_from="example.pdf")  

# 修改部分元数据  
writer.add_metadata(  
    {  
        "/Author": "Martin",  
        "/Producer": "Libre Writer",  
        "/Title": "Title",  
    }  
)  

# 清空所有数据但保留 PDF 中的条目  
writer.metadata = {}  

# 替换所有条目为新的一组元数据  
writer.metadata = {  
    "/Author": "Martin",  
    "/Producer": "Libre Writer",  
}  

# 将新 PDF 保存到文件  
with open("meta-pdf.pdf", "wb") as f:  
    writer.write(f)  
```  

## 删除元数据条目  

```python  
from pypdf import PdfWriter  

writer = PdfWriter("example.pdf")  

# 删除元数据（/Info 条目）  
writer.metadata = None  

# 将新 PDF 保存到文件  
with open("meta-pdf.pdf", "wb") as f:  
    writer.write(f)  
```  
