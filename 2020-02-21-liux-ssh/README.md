# SSH

这篇日志记录个人在系统性地学习 OpenSSH 相关工具过程中的一些零碎笔记，SSH 协议不在本篇日志的记录范围之内。

## [ssh](https://www.ssh.com/ssh)

大部分 Unix / Linux 系统都内置了 `ssh` 命令，用于在本机启动 SSH 客户端程序，与远程主机建立安全的连接。`ssh` 命令可以用于登录远程主机，在两台主机之间传输文件，以及在远程主机上执行命令。

使用 `ssh` 登录远程主机非常简单，常用形式如下：

```
ssh [-p port] [user@]hostname
```

使用 `ssh` 第一次连接到某台远程主机时，用户需要遵循提示，将这台远程主机的 host key 保存到本机的 `~/.ssh/known_hosts` 文件中。

使用 `ssh` 与远程主机建立连之后，用户需要输入密码进行身份认证。在身份认证成功之后，用户便会进入远程主机的 shell prompt（即等待用户输入 shell 命令的交互界面）。

---

`ssh` 也经常被用于这样的场景：无需登录远程主机进行 shell 交互，而是直接执行远程主机的 shell 命令。在这种场景中，可以使用如下形式的 `ssh` 命令：

```
ssh [-p port] [user@]hostname [command]
```

---

`ssh` 默认会从用户级别的 `~/.ssh/config` 文件和系统级别的 `/etc/ssh/ssh_config` 文件读取 ssh 配置数据，也可以使用 `ssh` 的 `-F configfile` 选项显示指定配置文件的地址。

`ssh` 命令行中的配置选项优先级最高，其次是用户级别的配置文件，再其次是系统级别的配置文件。`ssh` 配置文件的语法如下：

- 空格和以 `#` 开头的行会被忽略
- 每行以关键字开头，其后是所需的参数列表
- 配置选项之间可以使用空格或者等号分隔
- 可以使用双引号（`"`）将参数括起来，用于指定包含空格的参数

一个简单的 `ssh` 配置文件如下：

```
~$ cat ~/.ssh/config
# 阿里云
Host aliyun
    HostName 11.22.33.44
    User root
    Port 22
# 腾讯云
Host tencent
    HostName 55.66.77.88
    User app
    Port 22222
~$
```

## [ssh-keygen](https://www.ssh.com/ssh/keygen)

`man ssh-keygen` 摘要：

```
NAME
    ssh-keygen -- authentication key generation, management and conversion

DESCRIPTION
    ssh-keygen generates, manages and converts authentication keys for ssh(1).  ssh-keygen can create keys for use by SSH protocol version 2.

    The type of key to be generated is specified with the -t option.  If invoked without any arguments, ssh-keygen will generate an RSA key.
```

`ssh-keygen` 命令可用于生成、管理和转换 ssh 的认证密钥。`-t dsa | ecdsa | ed25519 | rsa` 参数可用于指定生成密钥的类型（默认值为 `rsa`），`-C comment` 参数可用于为密钥添加备注。

直接运行 `ssh-keygen` 命令，按照提示逐步操作，即可生成 id_rsa（密钥）和 id_rsa.pub（公钥）。

## [ssh-copy-id](https://www.ssh.com/ssh/copy-id)

## [ssh-agent](https://www.ssh.com/ssh/agent)

`man ssh-agent` 摘要：

```
NAME
    ssh-agent -- authentication agent

DESCRIPTION
```

## [ssh-add](https://www.ssh.com/ssh/add)

## [scp](https://www.ssh.com/ssh/scp)

## sftp

略

## [sshd](https://www.ssh.com/ssh/sshd)

略
