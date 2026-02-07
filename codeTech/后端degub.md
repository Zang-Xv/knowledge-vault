# 背景

记录一些关于我在 debug 过程中遇到的疑惑和迷思
顺便作为一个流程记录和速查用途

## traceback

许多代码的错误提示都是这样的 traceback, 但是很多时候我却看不明白, 因为我对于代码整体都缺乏一个认知, 所以感觉 traceback 都无从下手, 根本不知道是从哪里开始的

### 怎么看

Python 的 traceback 从上到下是「调用过程」，
真正的错误原因在最下面。
traceback 结构:

```bash
Traceback (most recent call last):
  File A, line x, in func1
    ...
  File B, line y, in func2
    ...
  File YOUR_CODE, line z, in your_func
    出错语句
异常类型: 异常信息
```

你的 traceback 最重要的是最后 3 行：

```bash
  File "/home/ubuntu/ChartSpark/utils.py", line 19, in clear_folder
    for filename in os.listdir(main_folder+folder_path[i]):
FileNotFoundError: [Errno 2] No such file or directory: 
'C:/Users/user/A-project/speak/output/mask/bar/background'
```

解析:
出错位置: File "/home/ubuntu/ChartSpark/utils.py", line 19, in clear_folder
出错的代码行: os.listdir(main_folder + folder_path[i])
异常类型: FileNotFoundError

## 异常与处理

我发现我甚至连抛出异常和处理的流程都不明白
