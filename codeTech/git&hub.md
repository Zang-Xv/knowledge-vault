# 云托管

随着代码和文件的反复迁移, 如何熟练使用 git 也需要提上日程并记录下来

## 什么是托管

## git 使用

[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
该博客提到编写教程的来由是因为现有教程不是难得令人发指，就是简单得一笔带过，这也符合我的感受，很难有初学者可以从零到一完整获得学习体验的博客可以阅读。
我将要先找一份一文读懂来试试水，然后再来评价这份教程如何、能否解决我的实际问题。

一文通确实不太行。但是在博客下面留言说能否对照ide的图形化界面来进行，这确实是不错的，如果没有的话我会自己进行这一步。
作为入门教程而言确实不错了，一步步教人如何使用，但是仅仅只说明了git命令行，其实更多还是服务器端教学
ide和各种gui其实有更多图形化的界面，如果能够结合图形化的界面说明就更好不过了

[一文带你彻底搞懂Git！--知乎](https://zhuanlan.zhihu.com/p/559692211)
扫了一眼，前面还有一些概念性的东西，后面就变成命令手册了，进阶一下的时候补充命令知识还是不错的，用来速查也可以，但是入门还是差点

### 问题

何为快照？有什么好处？
新文件中会继续使用与旧文件内容相同部分的磁盘空间，不同部分则写入新的磁盘空间。

合并与分支的定义和区分方式？

restore和reset的区别？
缓存区和本地仓库的交互关系到底是？

```bash
$ git status -s
 M README # 右边的 M 表示该文件被修改了但是还没放入暂存区
MM Makefile # 左边的 M 表示该文件被修改了并放入了暂存区；右边的 M 表示该文件被修改了但是还没放入暂存区
A lib/git.rb # A表示新添加到暂存区中的文件
?? LICENSE.txt # ??表示新添加的未跟踪文件
```

### 廖雪峰git教程

首先介绍了git的必要性，通过word文档使用上的一些困境进行了引入
然后简单介绍了一下git的发明历史，同时也进一步厘清其中的一些概念
不错的写作方式，可以学习作为：使用背景/使用必要性 发明历史和故事的分点博客写作纲要

一开始着重讲了git的版本管理功能，方便入门者上手
但对于有一定基础的知道大概用法，但仍然对git结构云里雾里的人来说不够看
不过好在很快就讲到工作区与缓存区的问题了
不得不说这个教程虽然是面向初学者，其实还是有一点代码门槛的

#### 什么是版本库

版本库就是仓库
每个被git init初始化过的路径都可以作为仓库
实际上这个说法有点问题，仓库应该就是用来管理版本的，所以才有译名称之为版本库，指的应当就是.git文件夹下git生成的相关文件
而工作区才应该是我们平时打开的文件夹，你可以在里面随意编辑，但是不会对仓库有影响，除非你commit，仓库也不会对工作区有影响，除非你pull或者checkout
而git会根据仓库内head指向的版本更改工作区内的文件
我的理解是大概正确的

Q：输入git add readme.txt，得到错误：fatal: not a git repository (or any of the parent directories)。
A：Git命令必须在Git仓库目录内执行（git init除外），在仓库目录外执行是没有意义的。

上面这个问答也某种程度印证了仓库就是.git文件夹的想法，不init就无法使用，但是如果在子目录的话大概是会递归查找到根目录的.git？等会可以尝试一下

本地仓库和远程仓库的区别只是个人仓库和中心化的管理服务器（通常使用github）罢了
git可以修改，还原版本库中的每个版本
难么暂存库的作用是？
add相当于生成快照？把文件的更改内容放入版本库（git）
但其实我还是没有太明白缓存区的根本作用在于哪里，难道说仅仅只是为了提供一个方便一次性更新多个文件到版本库的缓存区吗？毕竟多文件夹的文件添加会比较麻烦。似乎暂存区是通过索引实现的？

![git版本库](./assets/git版本库.png)
右侧版本库即为.git文件夹内的内容

#### 版本管理与版本树

文中提到了多次提交和暂存，但是它在图形化界面中对应上版本树会是怎样的？
我姑且猜测最左侧是主仓库版本，右侧是每个暂存内容，等会可以手动测试一下会有什么变化

同心圆代表merge
小圆点代表commit
但是为什么主分支上也有commit？这是通过init的用户决定的？

HEAD表示当前版本，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
--hard会回退到上个版本的已提交状态
--soft会回退到上个版本的未提交状态
--mixed会回退到上个版本已添加但未提交的状态
这里的区别在于？

#### 远程仓库

这里的远程仓库还涉及一些ssh key的问题
创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
后续再整理ssh的内容

为什么关联远程仓库需要命名，只是为了方便辨认？还是说可以同时连接多个远程仓库所以方便查找

#### 分支

为什么分支操作也用的是checkout，和回退文件是一样的
这二者的实现逻辑是相同的吗？
后面git又推出switch方便分支控制

merge后即便有冲突也已经是合并过后的了，只是冲突位置会有标记
具体情况应该尝试一下

合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
二者区别到底是？
根据[官方文档](https://git-scm.com/docs/git-merge)可知，--ff和--no-ff是在“当合并的历史是当前历史的后代”的情况才起作用的，下面具体介绍一下“当合并的历史是当前历史的后代”：

本地仓库和远程仓库在权限和使用上应该任然是等价的吗？push和commit等价吗？那么分配到任务做每个接口开发的人员push到远程仓库会发生到什么？
如果它在本地commit，那么个人分支就会增加一个版本。然后再push，个人分支依旧只会增加一个版本，因为push只是同步而已。
似乎是等价的，一般我们可以将一个master分支专门作为稳定的版本发布，一个dev分支作为测试分支，并主要由远程仓库的管理员管理，合并和发布都由管理员进行，方便统一管理。
如果在不需要更多代码的情况下，开发人员可以一直不pull新的仓库或者分支，这样本地其实只有个人分支一直更新。
但作为统一版本的远程仓库上却有所有分支的更新记录。

#### 缓存stash

这个缓存和暂存似乎不太一样，暂存切换了分支之后也会消失，属于纯本地的工作，缓存却是可以恢复的
可能是为了commit的提交都是可用的，完整的更新，不是半途而废，是可以被人拿去接着复用利用可以评估进度的罢
例如有大量琐碎的文档更新之类的, 就可以用stash在本地保存进度, 以便可以恢复和回退

当然也可以创建一个checkpoint分支, 专门用来随手储存, 当要主动提交一个完整改动的时候, 就合并到主分支或者dev分支上

##### 流程

创建新分支`git checkout -b ckpt(checkpoint)/wip(work in progress)/tmp/draft/exp`
之后每次想保存状态：

```bash
git add .
git commit -m "checkpoint: docs progress Jan18"
```

这些 commit 都在 checkpoint 分支，不会污染 main
切换到 main，并确保它是干净的, 然后再把 checkpoint 合并过去

```bash
git checkout main
git status
git merge checkpoint
git push origin main
```

checkpoint 很乱，但我只想要「一个干净提交」
推荐：squash merge（压缩提交）

```bash
# 切回 main
git checkout main
# 使用 squash 合并 checkpoint
git merge --squash checkpoint
```

这一步会：
把 checkpoint 分支的所有改动
压缩成 “一次尚未提交的修改”
不会引入 checkpoint 的提交历史

再手动写一个“正式 commit”: `git commit -m "feat: complete docs and config refactor"`
现在 main 的历史是：`feat: complete docs and config refactor`
而不是一堆零碎快照。

#### cherry-pick

注意：我们只想复制4c805e2 fix bug 101这个提交所做的修改，并不是把整个master分支merge过来。
为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支
这个指令的使用范围和创造原因有些迷，merge不够用吗？
而且也不知道在树上表现如何？应该是相当于复制了一个commit过来。
那么这个pick用处可能也是保证版本树上的清晰性，merge太多显得交织混乱，而且也可能引入一些神秘代码产生其他问题干扰开发，只用pick在树上就显得清晰干净多了

#### 变基

rebase操作可以把本地未push的分叉提交历史整理成直线；
rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。
Git 先把当前分支的 HEAD 指针回退到一个共同的起点（比如远程分支的最新提交），然后准备把你的提交“重播”到这个新基础上。
因为git实际上是通过保存文件的变化的快照来进行版本管理的，所以只要将变化应用到其他的变更上就可以很方便地实现这个过程，真是天才设想
而且这样就可以保持版本树的可读性和美观性，非常具有极客风的设计

#### 标签

感觉标签的作用更加近似于给版本打上标记，可以通过提交历史树获取更多的信息
而且本地标签和远程标签是不同步的，需要另外推送，这点有些奇怪，远程和本地怎么做了那么多的区分？意义为何？

为什么删除远程标签这么奇怪？使用的是推送本地已经删除的标签，也就是相当于推送了一个空标签？
git push origin :refs/tags/v0.9

#### github仓库

github的仓库也有点类似于git一样，相当于服务器上的不同权限管理的仓库
说得不明不白，但大概是这么个意思把

#### ignore

忽略文件的原则是：

- 忽略操作系统自动生成的文件，比如缩略图等
- 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
- 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

github上有各种配置的模板，可以方便使用，以免自己不知道会产生什么，要提交什么忽略什么，所以gitignore也要提交
也算是开源指导了，microsoft vscode有些插件就是作为开源的插件样例用于学习，他本身并不进行更新和维护

#### fetch

fetch和pull有什么区别？

fetch -> 取得
更新本地 git 的版本库, 获取远程仓库的更新
如果不 fetch 的话, 即使远程仓库更新了, 你在本地 `git status/log` 也是看不到的

pull -> 拉取(拉 + 取)
同时更新 git 版本库, 并进行 merge 操作
也就是进行了"取"的操作, 并且"拉"来合并(姑且这样理解区分吧)

但是如果本地有新的 commit 的话, 就会形成分叉
为了避免分叉, 一般都是使用 `git pull --rebase`
甚至于有直接将 rebase 设置别名, 直接当作普通的 pull 使用了

### git命令速记

```bash
# 配置个人信息
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

# 初始化仓库
git init

# 添加暂存
git add readme.txt

# 提交
git commit -m "xx"

# 查看仓库状态或者修改情况
git status
git diff

# 查看版本树
git log
# 简化输入为一行（其实简化后只剩id和commit_message，和图形化界面中看到的信息基本一致了）
git log --pretty=oneline

# 回退版本
git reset --hard HEAD^
# --hard会回退到上个版本的已提交状态
# --soft会回退到上个版本的未提交状态
# --mixed会回退到上个版本已添加但未提交的状态
# 撤销add，既将文件从暂存区退回工作区
git reset HEAD <file>

# 查看短时间（默认30天）内的本地命令记录
git reflog

# 重置工作区文件至上一次add或commit的状态
git checkout -- <file>

# 切换分支
git checkout

# 删除版本库中的文件，相当于对当前文件夹进行一次add
git rm <file>

# 连接远程仓库
git remote add origin 用户名连接git@github.com
# 查看远程仓库的详细信息
git remote -v

# 第一次推送推送到远程库
git push -u origin master

# 查看远程库信息
git remote -v

# 删除远程库连接
git remote rm origin

# 克隆仓库
git clone git@github.com

# 创建并切换分支
git checkout -b dev
git switch -c dev

# 等价于
git branch dev
git checkout dev

# 查看当前分支
git branch

# 合并指定分支到当前分支
git merge dev

# 删除分支
git branch -d dev

# 缓存当前工作区和暂存区
git stash
# 查看缓存
git stash list
# 恢复但不删除
git stash apply stash@{0}
# 删掉
git stash drop
# 恢复并删除
git stash pop

# 复制提交到当前分支
git cherry-pick 4c805e2

# 在当前分支打标签
git tag <name>
# 查看所有标签
git tag
# 用-a指定标签名，-m指定说明文字，id指定提交
git tag -a v0.1 -m "version 0.1 released" 1094adb
# 查看标签信息
git show <tagName>

# 推送标签到远程
git push origin <tagName> 
# 删除远程仓库中本地已经删除的标签（推送新的空标签？）
git push origin :refs/tags/v0.9
```

### commit信息模板

```text
<type>(<scope>): <subject>
<BLANK LINE>
<body> (可选)
<BLANK LINE>
<footer> (可选)
```

- type（类型）：说明修改性质
- scope（作用域，可选）：说明影响子模块或者子目录
- subject（标题）：一句话描述
- body（正文，可选）：详细原因、影响
- footer（可选）：关联 issue、BREAKING CHANGE 等

| type | 说明 |
| -------- | -------------------- |
| feat | 新增功能（feature） |
| fix | 修复 bug |
| docs | 文档修改 |
| style | 代码风格、格式调整（不影响功能 |
| refactor | 重构，既不是新增功能也不是 bug 修复 |
| perf | 性能优化 |
| test | 增加/修改测试 |
| chore | 构建、依赖、脚本等杂项 |
| revert | 回退 commit |

subject（标题）写法
简明扼要，祈使句，动宾结构
add, remove, update, correct等

### 快速同步备份项目

#### 前期修改

首先在 github 上创建好仓库
接下来修改一些相关配置

```bash
git --version
# 相关global配置的储存位置为 ~/.gitconfig
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"

# clone项目只会修改一些默认项目, 几乎没用
git init
# clone项目一般会报错已有origin
git remote add origin git@github.com:gitName/spark-repl.git

# 修改config内容 使用set-url
git remote set-url origin git@github.com:gitName/spark-repl.git
```

#### 编写gitignore

```gitignore
# ===== 系统文件 =====
.DS_Store
Thumbs.db

# ===== 编辑器 / IDE =====
.vscode/
.idea/
*.swp

# ===== 日志 =====
*.log
logs/

# ===== 环境 / 密钥 =====
.env
*.key

# ===== 构建产物 =====
dist/
build/
out/

# ===== 依赖 =====
node_modules/
vendor/

# ===== Python =====
__pycache__/
**/__pycache__/
*.py[cod]
.venv/
venv/

# ===== 临时文件 =====
tmp/
temp/
```

有时候 clone 项目本身 gitignore 不是很干净, 需要我们手动清除索引

```bash
# 从暂存区把所有文件的索引删除, --cached 不删除文件
git rm -r --cached .
# rm 后哪些应该ignore的文件会被标注D

git add .
git commit -m "Remove tracked __pycache__ and apply .gitignore"
```

#### 提交并推送

```bash
git status
git add .
git commit -m "init project"
git branch -M main
git remote add origin git@github.com:username/project.git
git push -u origin main
```

本地修改后

```bash
git status
git add .
git commit -m "xxx 描述"
git push
```

推荐的进阶小习惯（强烈建议）
提供 .env.example 但不要提供.env
`cp .env .env.example`
