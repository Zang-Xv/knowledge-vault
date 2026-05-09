# md转word介绍

使用 md 来进行 word 文档写作的方法

## 前置条件

vscode+markdown preview enhance 插件
安装 Pandoc

## 使用方法

[文档](https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/pandoc)
在对应的 md 文件首部写入 yaml

```yaml
---
output: word_document
---
```

可以填写更多参数实现更细致的控制

```yaml
title: "Habits"
author: John Doe
date: March 22, 2005
output:
  word_document:
    path: /Exports/Habits.docx
    reference_docx: myStyles.docx
    pandoc_args: ["--csl", "/var/csl/acs-nano.csl"]
```

如果不写对应的 reference_docx 路径, 将会在 `C:\Users\admin\AppData\Roaming\pandoc`路径下查找
但是文档当中说明的是`--data-dir`? 有些费解

编辑对应的 reference.docx 文件

可以右键转化

也可直接使用 pandoc 命令转化
pandoc input.md -o output.docx
