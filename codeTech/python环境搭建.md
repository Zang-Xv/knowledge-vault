# 环境搭建目的/背景
用于ai和计算机视觉相关的项目python环境搭建和隔离。
主要用于实验和分析，如果开发使用的话也许使用uv进行项目管理会更好一些。
其实也可以使用 conda（处理跨语言库）+uv（项目管理）兼顾二者的优点，但是对应的包安装，环境配置也需要进行conda和uv的双重设置。这里我只是进行较小型的实验用途，就不使用uv了。

## 环境配置
我使用的实验环境为VS Code + miniConda + Jupyter 插件
- VS Code：代码编辑器 + 调试器 + 插件平台
- miniConda：环境管理器 + 包管理器，创建独立的 Python 环境，自动处理复杂依赖
- Jupyter 插件：交互式运行支持（连接 Jupyter 内核）

## 搭建流程：
### 1. 安装基础工具
- 安装 VS Code
    - Python 插件（提供高亮和补全）、Jupyter 插件（交互式运行支持）
- 安装 Anaconda 或 Miniconda
    - 推荐 Miniconda，更轻量（官网下载安装包）
    > 这里简单选择miniconda而不是mamba/miniforge，毕竟实验用到的库都比较常见，使用anaconda官方维护的软件库也许会更加方便一些吧。如果有更多需求的话再转用mamba和miniforge也不迟。
    安装时一直 next 即可，注意不要添加到 PATH（否则可能会不论何时都优先使用conda的python环境，不利于各种项目的环境隔离，每次都需要手动退出base环境或者需要调整PATH优先级之类的）。

- 安装 Jupyter 及其内核
    - 提供交互式计算环境，便于修改参数进行实验和实验记录
    - 在 base 中运行：conda install jupyter
    > 一般在base环境安装 jupyter/jupyterlab 用于运行 notebook ，在虚拟环境内安装 ipykernel 用于注册内核，标识 notebook 该用哪个 Python 环境（或其他语言环境）来跑

### 2. 创建 Conda 环境（按课程/项目隔离）

- 不添加 PATH 时，怎么用 conda？
    - Anaconda Prompt（推荐）：安装 Miniconda 后，会自动给你一个特殊的终端快捷方式：Anaconda Prompt。打开它，就能直接使用 conda 命令。因为这个终端启动时，已经帮你临时把 Conda 的路径加到了 PATH。

    - conda init（更灵活）：如果更习惯用 PowerShell / CMD / VS Code Terminal，可以先运行一次：conda init powershell
    这样 Conda 会在这些终端的配置文件里自动加 PATH。从此以后，在这些终端里也能直接敲 conda。
    成功后会在对应终端的提示符前面显示 (base)，这说明 base 环境已经默认自动激活，此刻用的 Python 和包管理都走的是 Conda 的环境。![base激活示意](image.png)
        - 如果想要退出 conda 的 base 环境：conda activate base
        - 如果想要关掉自动激活：conda config --set auto_activate_base false
        - 如果想要把 conda init 修改的配置重置：conda init --reverse powershell


#### conda create -n myenv python=3.11 创建新的环境
> -n myenv → 新环境的名字叫 myenv
python=3.11 → 指定安装 Python 版本（如果不写，就跟随 base 环境的版本）
conda activate myenv → 激活环境
conda deactivate → 退出环境回到 base
conda remove -n myenv --all → 删除环境里的所有包和环境目录
conda create --prefix D:\yourpath\env python=3.11 → 将环境建立在指定路径，但：
>    - 激活时要写全路径，除非你给它加了名字


- 第一次访问官方仓库时，它会问你是否接受该源的 Terms of Service（服务条款）。主要是因为conda的仓库不允许商用，需要购买，但学习可以直接使用。![alt text](image-1.png)
但也可以在 install 命令后添加 -c conda-forge 来指定其他的开源/免费仓库源（channel）如：conda install numpy -c conda-forge。也可以在conda配置中修改源的优先级列表，这样就可以保证使用的仓库均为免费而不需要每次都进行指定了。

- 接下来将会安装一些Python 必须依赖的底层库（如 openssl、zlib）![python必须依赖的底层库](image-2.png)

- Conda 会在 D:\miniconda3\envs\myenv（默认）下新建一个文件夹，里面有：一个独立的 python.exe、独立的 site-packages（存放依赖库）

#### 注册Jupyternel内核
1. 怎么注册内核
在 base 环境中 conda install jupyter，在虚拟环境中 conda install ipykernel
在 conda 虚拟环境里执行：
```bash
conda activate myenv
python -m ipykernel install --user --name=myenv --display-name "Python (myenv)"
```
这一步就是 注册内核。
> --name=myenv → 给内核起 ID（技术层面）。
--display-name "Python (myenv)" → 在 Jupyter 界面显示的名字。
注册后，Jupyter 就能在“内核选择器”里找到这个环境。

2. 为什么需要注册内核？
Jupyter 本身不“知道”某个环境的 Python 解释器在哪。
注册后，Jupyter 会在用户配置目录下生成一个 kernel spec 文件（JSON），用来描述这个内核指向哪个解释器。
可以通过下列命令查看注册好的内核列表：
```bash
jupyter kernelspec list
```
3. Notebook 内核选择器的作用
当你点右上角 选择内核，或者在 VS Code 里执行 Notebook: Select Notebook Kernel：
其实就是在告诉 Jupyter：“这本 notebook 该用哪个 Python 环境（或其他语言环境）来跑”。
这样就能：不同 notebook 用不同的依赖环境，避免依赖冲突。

4. 注意：使用 vscode 的 Jupyter 插件会接管部分 原生的Jupyter 内核逻辑
vscode 的 python 扩展具有管理解释器的功能，它使得你能够在状态栏中看到当前环境的解释器是哪一个
在 notebook 的当前环境中安装 ipykernel 后，vscode 上的插件可以直接与内核通信，形成类似于手动注册的 python 内核
所以在使用 Jupyter 插件的时候，notebook 的右上角内核选择器中有两栏内核可用：python环境、jupyter kernel，这二者就对应插件自动通信并注册的内核，你手动输入指令识别的内核

#### 如何导出依赖（环境）
把某个环境中的依赖单独保存下来，别人就能通过这个来复现该项目。

Conda 官方方式：
```bash
conda activate myenv
conda env export > environment.yml # 会导出安装的所有包，顶层和间接依赖混在一起，也会再单独列出pip的所有包
conda env export --from-history > environment.yml  # 会导出你曾经手动安装过的顶层 conda 包，不包括pip包
```
导出所有安装包：
```yaml
name: myenv
channels:
  - defaults
dependencies:
  - bzip2=1.0.8=h2bbff1b_6
  - ca-certificates=2025.7.15=haa95532_0
  - expat=2.7.1=h8ddb27b_0
  - libffi=3.4.4=hd77b12b_1
  - openssl=3.0.17=h35632f6_0
  - pip=25.2=pyhc872135_0
  - python=3.11.13=h981015d_0
  - setuptools=78.1.1=py311haa95532_0
  - sqlite=3.50.2=hda9a48d_1
  - tk=8.6.15=hf199647_0
  - tzdata=2025b=h04d1e81_0
  - ucrt=10.0.22621.0=haa95532_0
  - vc=14.3=h2df5915_10
  - vc14_runtime=14.44.35208=h4927774_10
  - vs2015_runtime=14.44.35208=ha6b5a95_10
  - wheel=0.45.1=py311haa95532_0
  - xz=5.6.4=h4754444_1
  - zlib=1.2.13=h8cc25b3_1
prefix: D:\miniconda3\envs\myenv
```
只导出顶层包：
```yaml
name: myenv
channels:
  - defaults
dependencies:
  - python=3.11
prefix: D:\miniconda3\envs\myenv
```
因为 conda 导出的 YAML 文件不能区分哪些是项目直接依赖，哪些是间接依赖（需要通过另外安装tree来打印依赖树）。如果使用uv和pyproject.toml管理 Python 层依赖则会更加清晰，uv会更精确地记录依赖树。
可以通过二者结合使用来规避该缺点，但是conda和uv的结合使用起来相对比较复杂，这里只是一个比较小型的学习用项目，使用conda进行管理防止快语言库的依赖问题就可以了。没有那么强大的管理需求。所以我们继续沿用conda进行环境管理。

其他人用时，只需：
```bash
conda env create -f environment.yml
```
这比 pip freeze > requirements.txt 更完整，因为它包含 conda 的依赖树。
但如果你只需要 pip 项目格式，也可以用：（不过这样 conda 装的 C 库依赖没法被捕捉到，比如 numpy 背后的 mkl）
```bash
pip freeze > requirements.txt
```

#### 如何导出代码（py 文件）

课程作业/小项目通常有 .py 或 .ipynb 文件，建议和 environment.yml 一起打包。

最佳实践（和企业类似的方式）：

建一个项目文件夹：
```bash
MyProject/
├─ src/              # 存放 py 源码
├─ notebooks/        # 如果用 jupyter
├─ environment.yml   # 环境依赖
├─ README.md         # 项目说明
└─ data/             # 数据集（如不大）
```

用 Git/GitHub/Gitee 管理版本（推荐）
```bash
git init
git add .
git commit -m "Initial project setup"
```

这样代码和依赖文件都能共享，别人 clone 下来只要跑：
```bash
conda env create -f environment.yml
```

就能跑你的项目。

如果只是单机备份

直接压缩整个 MyProject/ 文件夹即可

别压缩整个 conda 环境目录（太大，没必要）

安装常用库：

conda install numpy pandas matplotlib jupyter


（如果要用 PyTorch/TensorFlow，可以再加）


### 3. 让 VS Code 识别你的 Conda 环境

打开项目文件夹

选择 左下角 Python 解释器 → 选中 my_course 环境

这样运行和调试都会使用该环境

5. 写 Notebook / 测试脚本

如果是探索性实验、画图、跑小模型 → 用 Notebook (.ipynb)

如果是完整作业、要调试 → 用 .py 文件 + VS Code 调试器

可以混合使用：先在 Notebook 验证思路，再整理到 .py 中。

6. 测试代码（推荐 pytest）

在环境里安装：

pip install pytest


在 VS Code 里启用测试框架，就能自动识别并运行 test_*.py 文件。

### 4. 使用jupyter时候需要注意的点
#### 导入单元未被执行
在使用JUpyter完成cv课程作业时，发现跳过第一个代码单元格运行第二个代码单元格出现了cv2未引入的错误
为什么会出现 `NameError: name 'cv2' is not defined`？

在 Jupyter 中：
1. 同一个 Notebook 的所有单元格共享 **同一个内核 (Kernel)**，变量、导入的模块、函数都保存在内存里。
2. 但是：不同的 Notebook 默认使用 **不同的内核实例**（除非你手动切换到同一个）。所以 A.ipynb 里导入的 `cv2`，不会自动在 B.ipynb 里生效。
3. 你看到 `NameError: name 'cv2' is not defined` 的原因：当前这个内核里还没有执行过 `import cv2` 的语句，或者你重启过内核（Restart）导致之前的状态清空。


#### Jupyter Notebook 变量作用域解析

**关键概念：共享内核**
1. **全局命名空间**：所有单元格共享同一个 Python 解释器进程
   - 在第21个单元格中定义的 `fruits = ["apple", "banana", "cherry"]`
   - 存储在全局命名空间中，对所有单元格可见

2. **执行顺序决定可用性**：
   - 只要第21个单元格被执行过，`fruits` 就存在于内存中
   - 后续任何单元格都可以访问和修改它
   - 即使物理位置在定义之前的单元格也能访问（如果执行顺序正确）

3. **变量持久化**：
   - 变量会一直存在，直到内核重启或被删除

- 单元格左边的数字 `[4]` 表示执行顺序
- 可以用 `%whos` 查看所有变量
- 重启内核或者使用`%reset`会清空所有变量

#### 另外一些Jupyter的魔法命令教学

Jupyter Notebook 支持许多“魔法命令”（Magic Commands），用于提升交互式编程体验。常见魔法命令如下：

- `%time`：测量单行代码的运行时间  
  示例：`%time x = sum(range(10000))`

- `%timeit`：多次运行代码并统计平均耗时  
  示例：`%timeit x = sum(range(10000))`

- `%matplotlib inline`：让 matplotlib 绘图直接显示在 Notebook 中

- `%whos`：显示当前内核中的所有变量信息

- `%pwd`：显示当前工作目录

- `%cd 路径`：切换当前工作目录  
  示例：`%cd D:/codefile`

- `%ls`：列出当前目录下的文件

- `%run script.py`：运行指定的 Python 脚本

- `!命令`：在 Notebook 中直接运行系统命令  
  示例：`!pip list`、`!dir`

- `%%time`/`%%timeit`：测量整个单元格代码的运行时间  
  示例：  
  ```python
  %%time
  for i in range(10000):
      pass
  ```

更多魔法命令可用 `%magic` 或 `%lsmagic` 查看全部列表。
