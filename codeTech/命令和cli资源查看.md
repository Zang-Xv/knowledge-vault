# linux 命令速查

## bash

```bash
compgen -c  # 会查询所有可用命令, 命令多了加载非常慢
compgen -c | grep xxx  # 所有包含 xxx 的命令
compgen -c | sort | less  # 所有可用命令(分页查看)
man xxx  # 详细手册(Linux)
```

### GPU

| 命令                       | 用途                                                | 示例输出/说明         |
| -------------------------- | --------------------------------------------------- | --------------------- |
| `nvidia-smi`               | GPU 基本状态（显存、温度、利用率、驱动、CUDA 版本） | 你已经用过            |
| `nvidia-smi -q`            | GPU 详细信息（性能状态、ECC、显存使用）             | 很多细节              |
| `nvidia-smi -L`            | 列出所有 GPU 名称和 ID                              | `GPU 0: RTX 3060 ...` |
| `nvidia-smi dmon`          | GPU 监控实时数据（带表格）                          | 适合性能调试          |
| `nvidia-smi topo --matrix` | GPU 拓扑关系（PCIe 通道 / NVLink）                  | 多 GPU 环境必备       |

### CPU

| 命令                                                                  | 用途                          |
| --------------------------------------------------------------------- | ----------------------------- |
| `lscpu` (Linux)                                                       | 查看 CPU 架构 / 核心 / 线程数 |
| `cat /proc/cpuinfo` (Linux)                                           | CPU 型号 / 频率 / 缓存        |
| `wmic cpu get name,NumberOfCores,NumberOfLogicalProcessors` (Windows) | CPU 型号与核心数              |
| `top` / `htop`                                                        | CPU 使用率、进程信息          |
| `free -h`                                                             | 内存使用情况                  |
| `vmstat 1`                                                            | 实时 CPU / 内存 / IO 监控     |

### 查找

| 命令       | 常用参数/用法                | 用途                       |
| ---------- | ---------------------------- | -------------------------- |
| `find`     | -name "str", -size +100M     | 遍历硬盘查找               |
| `locate`   | locate string                | 系统定期构建的查找数据库   |
| `updatedb` |                              | 手动更新 locate 查找数据库 |
| `grep`     | `grep -r "TODO" .`(递归查找) | 查找文件内容               |
| `which`    | `which python`               | 查找可执行程序的具体路径   |

### tmux

> Prefix = Ctrl + b  

#### 一、会话（Session）管理

| 命令                           | 快捷键     | 说明           |
| ------------------------------ | ---------- | -------------- |
| tmux                           | -          | 启动一个新会话 |
| tmux new                       | -          | 新建会话       |
| tmux new -s name               | -          | 新建并命名会话 |
| tmux ls                        | -          | 列出所有会话   |
| tmux attach                    | -          | 连接最近的会话 |
| tmux attach -t name            | -          | 连接指定会话   |
| tmux detach                    | Prefix + d | 从当前会话断开 |
| tmux rename-session -t old new | Prefix + $ | 重命名当前会话 |
| tmux kill-session -t name      | -          | 删除指定会话   |
| tmux kill-server               | -          | 关闭所有会话   |
| tmux switch-client -t name     | Prefix + s | 会话选择并切换 |

#### 二、窗口（Window）管理

| 命令                       | 快捷键        | 说明           |
| -------------------------- | ------------- | -------------- |
| tmux new-window            | Prefix + c    | 新建窗口       |
| tmux new-window -n name    | Prefix + c    | 新建并命名窗口 |
| tmux list-windows          | Prefix + w    | 窗口列表       |
| tmux rename-window name    | Prefix + ,    | 重命名当前窗口 |
| tmux select-window -t :num | Prefix + 数字 | 跳转到指定窗口 |
| tmux kill-window -t :num   | Prefix + &    | 关闭当前窗口   |

#### 三、面板（Pane）管理

| 命令                 | 快捷键         | 说明            |
| -------------------- | -------------- | --------------- |
| tmux split-window -h | Prefix + %     | 左右拆分 pane   |
| tmux split-window -v | Prefix + "     | 上下拆分 pane   |
| tmux list-panes      | Prefix + q     | 显示 pane 编号  |
| tmux select-pane -L  | Prefix + left  | 切换到左侧 pane |
| tmux select-pane -R  | Prefix + right | 切换到右侧 pane |
| tmux select-pane -U  | Prefix + up    | 切换到上方 pane |
| tmux select-pane -D  | Prefix + down  | 切换到下方 pane |
| tmux kill-pane       | Prefix + x     | 关闭当前 pane   |

#### 四、目标（target）语法（非常重要）

| 语法                | 含义                   |
| ------------------- | ---------------------- |
| session             | 会话名                 |
| session:window      | 指定窗口               |
| session:window.pane | 指定 pane              |
| :num                | 当前会话下的窗口编号   |
| .num                | 当前窗口下的 pane 编号 |

示例：

- `tmux attach -t work`
- `tmux select-window -t work:2`
- `tmux kill-p

### 命令输入技巧

巧用管道符, 可以省去手动的复制粘贴

| 命令    | 作用                    | 例子                         | 说明                         |
| ------- | ----------------------- | ---------------------------- | ---------------------------- |
| `\|`    | 输出 -> 输入 (文本流)   | cat file.txt \| grep "error" | 把 file 的内容传给 grep 搜索 |
| `xargs` | 输出 -> 参数 (执行动作) | cat files.list \| xargs rm   | 把 list 里的名字传给 rm 删除 |
| `$()`   | 输出 -> 字符串 (嵌入)   | echo "Today is $(date)"      | 把 date 的结果拼接到 echo 中 |

### 其他命令速查手册

| 命令 | 含义      | 用途                  |
| ---- | --------- | --------------------- |
| `df` | disk free | 查看磁盘使用情况      |
| `ln` | link      | 创建链接, -s 为软链接 |

## 怎么看命令语法构成和 man

大部分都遵循同一套 Usage / Synopsis 语法规则。
`command [OPTIONS] [ARGUMENTS]`

规则简单说明

- `[]` —— 可选的
- `<>` —— 必填的
- `...` —— 可以重复多次
- `大写词（OPTION / FILE / DIR / COMMAND）`——占位符
- `|` —— 互斥选择（OR）, 二选一 / 多选一
- `=` —— 选项-参数绑定, 除了等号以外还有空格绑定, 紧贴绑定, 其实就是说明选项的参数是什么
- `{ }` —— 枚举集合, 例如说明参数值只能是这几个之一

### --help

这是 GNU 风格（Linux 常用）的设计, 通常大部分工具都会遵守这一点, 极力推荐支持 --help 和 --version
输入 --help 会打印出长篇大论的详细说明

以`curl --help`为例

```text
Usage: curl [options...] <url>
 -d, --data <data>          HTTP POST data
 -f, --fail                 Fail silently (no output at all) on HTTP errors
 -h, --help <category>      Get help for commands
 -i, --include              Include protocol response headers in the output
 -o, --output <file>        Write to file instead of stdout
```

先看第一行 Usage 速览结构
再看选项区
`-d`: 短选项, 作为缩写, 和长选项等价
`--data`: 长选项
`<data>`: 参数

### man

部分命令是没有 --help 可以看的, 不过输入 --help 会触发错误参数处理打印出 usage
这是 SD / Unix 传统风格：更加“高冷”和极简。认为“你应该去读手册（Man Page）”，而不是指望命令行打印一堆字。

先只看 SYNOPSIS / Usage 速览结构和用法

```text
SYNOPSIS
       grep [OPTION]... PATTERN [FILE]...
```

然后明确问题选用关键词, 使用`/`进行关键字检索, 查看手册的相关说明

查找到相关的内容或者手册的更多分页章节再进行跳转, 一般标注了`(x)`的就是手册页面了

| 章节 | 标记  | 含义         | 你什么时候用   |
| ---- | ----- | ------------ | -------------- |
| 1    | `(1)` | **用户命令** | 你平时敲的命令 |
| 2    | `(2)` | 系统调用     | C / 内核       |
| 3    | `(3)` | 库函数       | C 标准库       |
| 5    | `(5)` | 文件格式     | 配置文件       |
| 7    | `(7)` | 杂项 / 约定  | 协议、规范     |
| 8    | `(8)` | 管理命令     | root / 运维    |

日常使用一般只有`(1),(5),(8)`
可以使用`man 5 passwd/man passwd.5`进入(5)章节, 也可以直接在 man 的界面内将光标移动到对应位置按`Enter`跳转

### 各类教程

其实各类教程说明的都比较完整易懂, 而且自带翻译, 中文更是易读得多
这里以菜鸟编程的 head 说明为例

```text
head
取出文件前面几行

语法：

head [-n number] 文件 
选项与参数：

-n ：后面接数字，代表显示几行的意思
[root@www ~]# head /etc/man.config
默认的情况中，显示前面 10 行！若要显示前 20 行，就得要这样：

[root@www ~]# head -n 20 /etc/man.config
```
