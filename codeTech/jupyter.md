# jupyter有什么用
本质上只是 VS Code 的一个界面集成器。
作用：

- 在 VS Code 中提供 Jupyter Notebook 的交互式界面（.ipynb 文件的编辑、代码单元格运行、变量查看、图表展示）。
- 提供和 Jupyter Server 通信的桥梁。
- 提供快捷键、代码高亮、交互式输出等 VS Code 风格的体验。

但是插件 不自带 Jupyter 内核，它只是“前端”。需要另外安装Jupyter内核. 如果不再python环境中使用的话, 需要保证运行环境中可以调用 Jupyter 的内核.

# Jupyter 和 Ipython
## IPython 是什么？
Interactive Python，最早是一个 Python 的交互式命令行工具。
功能比 python REPL 强大：支持自动补全、魔法命令（%time, %run）、更好的调试交互。
它本质是一个 交互式 shell。
后来，IPython 开发团队觉得这种「交互式计算」不仅对 Python 有用，对 R、Julia、SQL 也有用，于是把项目扩展 → 产生了 Jupyter。

## Jupyter 是什么？
Julia + Python + R → 取三个语言的名字组成 Jupyter。
它是一个「多语言交互式计算环境」，最常见的就是 Jupyter Notebook。
Jupyter 的核心概念是：内核（Kernel）
Python 内核 = 实际就是 IPython 内核。
但你也可以换成 R 内核、SQL 内核、C++ 内核等等。