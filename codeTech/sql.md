# 环境配置
我这里尝试使用 vscode+jupyter notebook + SQLite 来进行学习和实验, 配置环境的话有两种方式, 但其实差别不算大
方法一: 使用 sql 内核(如xeus-sql), 直接运行notebook
方法二: 可以用 Python 的 sqlite3 模块或 SQLAlchemy 来连接数据库，在 Notebook 中执行 SQL。
方法三: 也可以用 ipython-sql 插件（pip install ipython-sql），这样你就能在 cell 里直接写：
```
%%sql
SELECT * FROM students;
```

## 不同数据库的区别
### SQLite
#### 主要特性
- 轻量级
不需要安装独立的数据库服务。
数据就是一个 .db 文件（比如 example.db）。
复制这个文件就等于复制了数据库。

- 零配置
系统自带（Linux/MacOS 默认有，Windows 安装 Python 也带）。
不需要启动或停止服务，直接用命令行或 API 连接即可。

- 兼容 SQL
支持标准 SQL（SELECT、INSERT、UPDATE、DELETE 等）。
但部分高级功能（用户权限、并行事务、存储过程）不支持。

- 事务支持
支持 ACID（原子性、一致性、隔离性、持久性）。
但并发写入有局限（写操作是串行的）。

- 广泛应用
浏览器（Chrome、Firefox）、手机（iOS/Android App）、嵌入式设备都在用 SQLite。


## 方法一: 使用sql内核的jupyter notebook
### 一、安装前提
已安装 Python 3.8+（推荐用 conda 或 venv 管理环境, 这里我使用的是conda），用于运行jupyter notebook

已安装 JupyterLab 或 Jupyter Notebook 的 python 环境(全局环境或者虚拟环境其一有即可), 用于运行jupyter server
如果还没有，可以先装：conda install jupyterlab

已安装 jupyter 插件的 vscode , 用于提供可交互界面便于编辑 .ipynb 文件

已安装 SQLTools 插件和对应的 SQLTools driver 插件， 用于方便对数据库进行操作和可视化

### 二、安装 SQL 内核（推荐 xeus-sql）
xeus-sql 是 Jupyter 官方推荐的 SQL 内核，支持 SQLite、MySQL、PostgreSQL 等。

1. 新建一个环境（比如叫 sql-env）
```bash
conda create -n sql-env 
conda install -c conda-forge xeus-sql soci-sqlite
```
如果未在全局python环境中安装jupyterlab的话, 则需要在该环境下安装jupyterlab, 否则只安装内核和soci包即可

3. 激活环境
conda activate sql-env
现在你就有了一个带 SQL 内核的 Jupyter 环境。

### 三、创建SQLite数据库
由于我这里是使用 conda 创建的 python 虚拟环境，所以没有自带 sqlite3 （一般不用装，系统自带，如果没有：sudo apt install sqlite3），需要安装：`conda install -c conda-forge sqlite -y`

1. 直接创建一个 .db 文件(最直接)

1. 命令行方式: `sqlite3 example.db`
进入后你会看到提示符：`sqlite>`
退出：`sqlite> .exit`

### 四、启动 Jupyter 并指定数据库
在环境内创建一个.ipynb文件, 打开即可启动  

在启动 Notebook 时指定连接的数据库，比如：  
```bash
%LOAD sqlite file.db # 若不在同一根目录需要指定路径
%LOAD mysql mysql://user:password@localhost/dbname
%LOAD postgres postgresql://user:password@localhost/dbname
```
### 四、测试运行 SQL
在新建的 Notebook 里输入：
```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT,
    age INT
);

INSERT INTO students (name, age) VALUES ("Alice", 20);
INSERT INTO students (name, age) VALUES ("Bob", 22);

SELECT * FROM students;
```
注意！！jupyter notebook 只能够同时运行一个语句，多个语句放在同一个单元格时只会执行第一句
运行后，你应该能直接看到结果表格。  
```
2 rows in set (0.00 sec)
id	name	age
1	Alice	20
2	Bob	22
```

### 五、在SQLTools中连接数据库(可选)
如果你希望有更加清晰的表格可视化界面可以观察数据库的整体情况, 那么可以安装SQLTOOLS和对应dirver, 但是插件稍微有一些延迟, 信息的更新可能会久一点  
在插件页面进行数据库的连接, 填入对应的信息即可
