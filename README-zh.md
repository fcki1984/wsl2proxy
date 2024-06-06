# WSL2Proxy

本仓库中的 Bash 脚本用于配置 WSL2 环境，以便让 http、https 连接和 git 客户端能够正确访问 Windows 下的代理服务器。

# 适用场景
如果代理ip固定，可以设置自启动，否则每次代理ip发生变化，都需要再次运行`wsl2proxy setup`

# 如何使用

## 安装

### 一键脚本

执行下面的命令来安装最新脚本：

```bash
# 中文版
wget -O w2p.sh -q https://git.io/JJvfD && sudo bash w2p.sh install

# 英文版
wget -O w2p.sh -q https://git.io/JJvf1 && sudo bash w2p.sh install
```

### 手动安装

你可以手动克隆并安装脚本：

```bash

$ git clone https://github.com/wizcas/wsl2proxy.git
$ cd wsl2proxy
# 中文版
$ sudo bash ./wsl2proxy-zh install
# 英文版
$ sudo bash ./wsl2proxy-en install

```

### 设置代理服务器

在激活 WSL2Proxy 之前，必须首先进行代理服务器的相关设置，包括：

-   局域网IP
-   代理协议
-   代理端口


执行下面的命令进行代理服务器的设置：

```bash
$ wsl2proxy setup
```

> 代理服务器设置被保存在`~/.wsl2proxy.conf`。

## 启用/禁用 WSL2Proxy

### 手动启用

执行下面的命令 (**别漏了命令开头的`.`，否则环境变量不会应用到当前的环境中**):

```bash
. wsl2proxy on
```

### 手动禁用

执行下面的命令 (**别漏了命令开头的`.`，否则环境变量不会应用到当前的环境中**):

```bash
. wsl2proxy off
```

### 随 WSL2 启动

在你的 Shell 启动脚本中 (`.bashrc`, `.zshrc`等) 添加这行代码:

```
. wsl2proxy on
```

### SSH 代理

脚本会在 home 目录下自动生成一个名为`socksproxy`的辅助可执行脚本，用来快速设置 SSH 代理。

比如说我想让所有访问`github.com`的 SSH 连接都通过代理（在使用`git@github.com:...`这样的地址克隆仓库时走代理了），
只需要编辑`~/.ssh/config`文件并添加下面的配置：

```
Host github.com
    User git
    ProxyCommand ~/socksproxy '%h %p'
```

**注意** 本脚本不负责自动修改你的`.ssh/config`文件，所以你必须自己正确处理该文件的权限。另外，如果让 SSH
走代理了，你也需要手动移除`config`文件中的相关配置。

## 卸载

1. 如果设置了随 WSL2 自动启用代理，请删除 Shell 启动脚本中的 `. wsl2proxy on` 这行。
2. 如果修改了 `~/.ssh/config` 文件，请删除其中相关的代理配置。
3. 删除文件 `/usr/local/bin/wsl2proxy`。
