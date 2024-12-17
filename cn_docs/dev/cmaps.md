# CMaps

查看 "crazyones" 的 cmap：

```bash
pdftk crazyones.pdf output crazyones-uncomp.pdf uncompress
```

你会看到以下内容：

```text
begincmap
/CMapName /T1Encoding-UTF16 def
/CMapType 2 def
/CIDSystemInfo <<
  /Registry (Adobe)
  /Ordering (UCS)
  /Supplement 0
>> def
1 begincodespacerange
<00> <FF>
endcodespacerange
1 beginbfchar
<1B> <FB00>
endbfchar
endcmap
CMapName currentdict /CMap defineresource pop
```

---

## codespacerange

`codespacerange` 将完整的字节序列映射到一系列 Unicode 字形。  
它定义了一个起始点：

```text
1 beginbfchar
<1B> <FB00>
```

这意味着 `1B`（十六进制 27）映射到 Unicode 字符 [`FB00`](https://unicode-table.com/en/FB00/) - 即连字符 **ﬀ**（两个小写字母 f）。

`begincodespacerange` 中的两个数字表示范围的起始偏移量为 0（因此从 `1B ➜ FB00`），直到偏移量 `FF`（十进制 255）。因此，`1B + FF = 282` 映射到 Unicode 字符 [`FBFF`](https://www.compart.com/de/unicode/U+FBFF)。

在文本流中，存在以下内容：

```text
(The)-342(mis\034ts.)
```

其中 `\034` 是八进制，表示十进制的 28。