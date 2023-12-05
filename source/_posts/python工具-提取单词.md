---
title: python批量提取整理单词
date: 2023-03-29 21:18:34
tags:
- 考研英语
- python
- 正则表达式
categories: 
- 笔记
---

> **提取单词**

本篇代码笔记介绍了如何从文件中提取单词并按字典序排序。通过使用 Python 的 re 模块和一些字符串处理函数，可以将文件中的单词提取出来，然后去重、排序，最终得到一个按字典序排序的单词列表。本文提供了一个完整的代码示例，可以直接使用或参考。
<!-- more -->
```python
import re
def extract_words_and_sort_from_file(file_path):
    # 打开并读取文件内容
    with open(file_path, 'r', encoding='utf-8') as file:
        content = file.read()
        words = re.findall(r'^\s*([a-zA-Z, ]+)$', content, flags=re.MULTILINE)
        words = ''.join(words)
        # 将逗号替换为空格，方便后续提取单词
        words = words.replace(',', ' ')
        words = re.findall(r'\b[a-zA-Z]+\b', words)
        words = [word.lower() for word in words]
        # 对单词列表去重并排序
        words = sorted(set(words))
        word_count = len(words)
        # 将单词列表连接为一个字符串
        return ', '.join(words), word_count

file_path = '../英语/顽固词汇.md'
result, word_count = extract_words_and_sort_from_file(file_path)
print("Sorted words:", result)
print("Word count:", word_count)
```
