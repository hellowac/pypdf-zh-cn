# 文本提取的后处理

后处理可以显著提升文本提取的结果。然而，这超出了 pypdf 本身的范围，因此库本身并不直接支持这一功能。后处理属于自然语言处理（NLP）任务。  

本页列举了一些可以进行后处理的示例，以及一个可用作通用后处理步骤的社区方案。如果你对文档的特定领域（例如语言）了解更多，那么很可能能够找到在你的上下文中更有效的定制解决方案。

---

## 连字替换

```python
def replace_ligatures(text: str) -> str:
    ligatures = {
        "ﬀ": "ff",
        "ﬁ": "fi",
        "ﬂ": "fl",
        "ﬃ": "ffi",
        "ﬄ": "ffl",
        "ﬅ": "ft",
        "ﬆ": "st",
        # "Ꜳ": "AA",
        # "Æ": "AE",
        "ꜳ": "aa",
    }
    for search, replace in ligatures.items():
        text = text.replace(search, replace)
    return text
```

---

## 去除连字符

连字符常用于分割单词，以使页面排版更美观。

```python
from typing import List


def remove_hyphens(text: str) -> str:
    """
    注意：该方法在以下情况下可能会失败：
    * 自然的短横线：well-known, self-replication, use-cases, non-semantic,
                    Post-processing, Window-wise, viewpoint-dependent
    * 数学运算符：2 - 4
    * 专有名词：Lopez-Ferreras, VGG-19, CIFAR-100
    """
    lines = [line.rstrip() for line in text.split("\n")]

    # 找到结尾带连字符的行
    line_numbers = []
    for line_no, line in enumerate(lines[:-1]):
        if line.endswith("-"):
            line_numbers.append(line_no)

    # 替换连字符
    for line_no in line_numbers:
        lines = dehyphenate(lines, line_no)

    return "\n".join(lines)


def dehyphenate(lines: List[str], line_no: int) -> List[str]:
    next_line = lines[line_no + 1]
    word_suffix = next_line.split(" ")[0]  # 获取下一行的首个单词

    # 将当前行与下一行首个单词拼接
    lines[line_no] = lines[line_no][:-1] + word_suffix
    lines[line_no + 1] = lines[line_no + 1][len(word_suffix) :]
    return lines
```

## 页眉/页脚移除

以下用于移除页眉/页脚的方法存在一些缺陷：

- **误判（False-positives）**：例如，对于第一页，如果有日期如 2024，可能会被错误移除。
- **漏判（False-negatives）**：许多情况下无法正确处理：
  - 页眉中包含动态内容，例如页码标签。
  - 偶数页和奇数页的页眉不同。
  - 某些页面（例如首页或章节页）可能没有页眉。

```python
def remove_footer(extracted_texts: list[str], page_labels: list[str]):
    def remove_page_labels(extracted_texts, page_labels):
        processed = []
        for text, label in zip(extracted_texts, page_labels):
            # 检查并移除左侧的页码标签
            text_left = text.lstrip()
            if text_left.startswith(label):
                text = text_left[len(label):]

            # 检查并移除右侧的页码标签
            text_right = text.rstrip()
            if text_right.endswith(label):
                text = text_right[:-len(label)]

            processed.append(text)
        return processed

    extracted_texts = remove_page_labels(extracted_texts, page_labels)
    return extracted_texts
```

---

## 其他建议

- **单位之间的空格**：数字和其单位之间应有一个空格。  
  （[来源](https://tex.stackexchange.com/questions/20962/should-i-put-a-space-between-a-number-and-its-unit)）。  
  例如：`42 ms`，`42 GHz`，`42 GB`。

- **百分号**：英语书写规范建议数字后直接跟随百分号，中间不留空格（例如：`50%`）。

- **句点前的空格**：通常应移除句点前的多余空格。

- **句点后的空格**：通常应在句点后添加空格以保持可读性。