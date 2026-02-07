# venv 的特点
- 自带 Python：venv 只是用来复制一份你本机 Python，形成独立环境。
- 隔离范围：只隔离 Python 包（site-packages），不管理系统库或 C/C++ 动态库。
- 包安装方式：依赖 pip，从 PyPI 安装。
- 如果包依赖底层 C 库（如 NumPy、SciPy、PyTorch），pip 可能需要自己编译或依赖系统库版本。

适合：基础 Python 开发、小型脚本、课程作业。

# Conda 的特点
- 独立管理 Python：每个环境可以有不同 Python 版本，不必依赖全局 Python。
- 隔离范围更广：
    - 不仅隔离 Python 包，还能隔离 底层库和动态链接库（C/C++ 库、CUDA、cuDNN 等）。
    - 例如 PyTorch GPU 版本依赖特定 CUDA 版本，Conda 会自动安装对应库。
- 包安装方式：通过 Conda 包管理器，从 Conda 仓库安装预编译好的二进制包（不需要本地编译）。

适合：科学计算、深度学习、多语言/复杂依赖环境。

# 为什么 Conda 在复杂依赖下更稳？
- 底层依赖处理

venv + pip 安装 PyTorch GPU 时，你可能需要手动确保 CUDA、cuDNN 版本匹配，否则运行报错。
Conda 会直接给你配好兼容版本的库和 Python，几乎无需手动干预。

- 跨平台兼容性好

Conda 二进制包针对 Windows、macOS、Linux 做好编译和依赖打包，pip 有时需要自己编译 C 扩展。

- 环境复现简单

Conda 有 conda env export > environment.yml，直接导出完整环境（包括 Python、C 库版本）。
venv 只能用 pip freeze > requirements.txt，不能保证底层库版本一致。

# 关于Conda & miniconda & miniforge & Mamba
这是conda生态下的一系列相似但不太相同的概念
conda：包含conda命令和其他常用的跨语言软件库
miniconda：只包含conda命令和其依赖，默认从官方软件库仓库中下载库
miniforge：miniconda的民间维护版，会默认从民间开源软件仓库中下载库
mamba：conda的开源版本，并且使用c++重写了解析器，速度更快
更多详情见：[隔壁的程序员老王](https://www.bilibili.com/video/BV1Fm4ZzDEeY)
