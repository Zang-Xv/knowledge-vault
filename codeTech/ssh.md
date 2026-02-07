# 关于 ssh

SSH（Secure Shell）是网络通信、远程管理、文件传输的核心技术之一，尤其在开发、运维、服务器管理等场景中几乎无处不在。  
所以我们可以常常见到 ssh 用在各种远程控制中, 这里将简要介绍一些有关 ssh 的概念, 并说明我使用 ssh 实现一些功能的过程

## ssh 的定义

SSH，全称 Secure Shell（安全外壳协议），是一种用于在 不安全网络中安全地访问远程主机 的加密网络协议。
它的主要用途包括：

- 远程登录到服务器（替代 Telnet）
- 执行远程命令
- 进行安全的文件传输（SCP、SFTP）
- 实现安全隧道或端口转发

它是一个 应用层协议，基于 TCP 传输层（默认端口号是 22），能够提供：

- 身份验证（确保你连接的是谁）
- 加密通信（防止中途窃听）
- 完整性校验（防止数据被篡改）

SSH 的衍生工具和命令:

| 工具           | 作用     | 说明               |
| ------------ | ------ | ---------------- |
| `ssh`        | 登录远程主机 | 最基本功能            |
| `scp`        | 文件复制   | 基于 SSH 进行加密传输    |
| `sftp`       | 文件管理   | 更强大的交互式文件传输协议    |
| `ssh-keygen` | 生成密钥   | 用于创建公钥和私钥        |
| `ssh-agent`  | 密钥管理   | 管理多个私钥会话         |
| `ssh-add`    | 添加密钥   | 将私钥加载进 ssh-agent |

---
这部分内容应该在计网的课程中有讲过, 包括 ssh 协议的组成, 以及跟 Telnet 等不安全协议的对比等  
但是我完全忘记了😅

## scp(secure copy) 远程文件传输

### 相关介绍

使用 scp 的原因是想要在局域网内快速传输文件到另一个 pc 上, 但是又不想掏出U盘/登录微信, 于是选择用这种方式进行文件传输  
scp 是基于 SSH 的简单文件复制工具（一次性拷贝，非交互式文件会话）。

### 使用前提(基于windows)

#### 1. 安装 OpenSSH

OpenSSH 就是使用 SSH 协议的一个开源实现, 一个具体应用, 它提供了客户端和服务端两个部分:  

- ssh/scp/sftp/ssh-keygen/ssh-agent 等工具（客户端）
- sshd（SSH 守护进程，服务器端）

一般 win 都有自动安装 SSH服务器 , 但是可能没有启动该服务  
而 SSH客户端 则需要手动安装  
其中分为命令行方法和图形化的安装方法, 不过二者实际上是同样的操作, 并且由于最后还是需要使用命令行来进行执行文件传输, 所以这里只介绍命令行(powershell)方法  

1. 查看启用和安装情况
`get-service -name *ssh*`
2. 如果没有`ssh-agent`和`sshd`的服务名说明还没有安装
使用`Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`
3. 将状态是 Stopped 的服务打开
`Start-Service sshd`
可以使用`Set-Service -Name sshd -StartupType 'Automatic'`来设置为开机自动启动, 否则每次都需要手动启用服务
4. 检查是否在监听22号端口, 如果状态显示为Listening就是正在监听
`netstat -an | findstr :22`
5. 网络/防火墙允许 TCP 22（或自定义端口）的入站连接。
使用`ping`检测网络是否允许通过
使用`Test-NetConnection -ComputerName 192.168.x.x -Port 22`检测能否访问到ssh服务

#### OpenSSH client 与 SSH service（sshd）是什么

ssh client 相当于是客户端的软件, 是主动发起连接的一方，不需要一直运行服务，只要安装了就能用。  
功能：在本地发起 SSH 连接（远程 shell、执行远程命令、上传/下载文件等）。常见命令：ssh, scp, sftp, ssh-keygen, ssh-agent, ssh-add。

SSH Service（常称 sshd，SSH Daemon / SSH Server）相当于是服务器的守护进程, 是被动的一方，需要作为后台服务运行并监听端口（通常是 22）。  
功能：在“被远程访问的机器”上运行，等待并接受来自 client 的连接请求，验证并创建安全会话，启动远程 shell 或处理文件传输请求（scp/sftp）。

### 使用方法

将本地文件拷到远端：

```bash
scp /path/to/localfile user@host:/path/to/remotedir/
```

将远端文件拷到本地：

```bash
scp user@host:/path/to/remotefile /local/dir/
```

递归复制目录（保留目录结构）：

```bash
scp -r /local/dir user@host:/remote/dir
```

指定端口：

```bash
scp -P 2222 file user@host:/path
```

指定私钥：

```bash
scp -i ~/.ssh/id_rsa file user@host:/path
```

注意：scp 会使用 SSH 的认证和加密机制（所以能安全传输）。不过对于大文件/增量同步更推荐 rsync -e ssh 或 sftp / sshfs。

## ssh远程连接主机

我在想能不能通过ssh远程连接个人主机当作服务器使用，同时不干扰本机原来的运行
也就是将终端输出通过ssh连接外显在另一个终端上，达到一种多窗口运行的效果

### 准备工作

必须准备包括
在客机上安装ssh client并运行
在主机上安装sshd并监听22端口

可选准备
保存ssh key以便后续连接不用每次都输入密码
打开设置中的 `Remote.SSH: Show Login Terminal`, 以便显示客机(登录终端)的相关信息, 可以方便调试

### 免密连接

需要把自己的公钥内容发送到别的机器的 authorized_keys 中，并在本机 config 文件中配置私钥文件位置

```bash
ssh-keygen -t rsa -b 4096
# -t Type（类型）rsa 指定要生成的密钥类型为 RSA
# -bBits（位数）4096指定密钥长度为 4096 位
# -f (file) 指定保存文件名 默认保存为 ~/.ssh/id_rsa
```

生成密钥对的时候要求输入 passphrase 这是额外的密码, 用于保护密钥对, 个人使用一般不需要
直接回车不使用 passphrase 即可

key's randomart image 是个辅助确认密钥变化的图像, 视觉要素更容易识别
还有其他用处这里暂时不表

修改 Ubuntu 系统 /etc/ssh/sshd_config  配置文件，添加上以下配置信息：

RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile      .ssh/authorized_keys

完成以上步骤后，重启SSH服务以应用更改：

### 内网连接

[参考博客](https://www.cnblogs.com/emanlee/p/18807002)
在remote ssh 插件中进行配置, 保存连接信息
输入密码就可以连接

#### 问题

究竟怎么确认自己电脑的用户名和密码呢?
激活系统的时候需要先创建或者登录微软账号
如果此时没有跳过, 则为微软账号和微软密码
如果激活时跳过了, 则为本地账号和密码

除此之外还有 windows 系统对于账户`名称`和`全名`的神秘区分问题
[账户名称和全名的混淆问题](https://blog.csdn.net/weixin_49371288/article/details/131823759)

命令行输入`whoami`
输出：计算机名\本地用户名
可以在 设置 → 账户 → 登录选项 中修改密码。

### 外网连接

使用 D-DNS 可以将 ip 绑定到域名上, 这样就可以解决家用 ip 更新的问题
但是由于是内网 ip 如果不在局域网内就没办法轻松访问了, 需要一些跨域的配置

我能想到的办法
内网穿透(不知道怎么配置)
公网ip转发(需要服务器或者公网ip)

#### 内网穿透

|工具|特点|适用场景|
|---|---|---|
|FRP|开源、自建服务器|需要一台公网服务器|
|ZeroTier|P2P 虚拟局域网|无需服务器，免费|
|Tailscale|WireGuard 简化版|易用，免费额度大|
|cpolar|国内商业方案|有免费套餐|

市面上主流的穿透工具还有Ngrok, Natapp, 花生壳, Ssh, autossh, lanproxy, Spike
