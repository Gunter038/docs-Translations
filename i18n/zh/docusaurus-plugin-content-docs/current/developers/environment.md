- - -
sidebar_label : 设置环境
- - -

# 开发环境

本教程将介绍如何设置您的开发环境以运行 Celestia 软件。 此环境可用于开发、构建二进制文件和运行节点。

## 安装依赖项

设置好主机后，通过 ssh 进入主机以开始安装运行节点所需的依赖项。

首先，确保更新和升级操作系统：

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

这些是执行许多任务（如下载文件、编译和监控节点）所必需的基本包：

<!-- markdownlint-disable MD013 -->
```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## 安装 Golang

Celestia-app 和 celestia-node 是用[Golang](https://go.dev/)编写的，所以我们必须安装 Golang 来构建和运行它们。

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

现在我们需要将 `/usr/local/go/bin` 目录添加到 `$PATH`

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

要检查 Go 是否安装正确，请运行：

```sh
go version
```

输出应该是已安装的版本：

```sh
go version go1.18.2 linux/amd64
```
