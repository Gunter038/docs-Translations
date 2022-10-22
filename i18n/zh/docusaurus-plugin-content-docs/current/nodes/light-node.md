---
sidebar_label: Light Node
---

# 设置 Celestia 轻节点

本教程将指导您设置 Celestia 轻节点，这将允许您在数据可用性 (DA) 网络上执行数据可用性采样。

> 要查看设置 Celestia 轻节点的视频教程，请单击[此处](../developers/light-node-video.md)

## 轻节点概述

轻节点确保数据的可用性。 这是与 Celestia 网络交互的最常见方式。

![light-node](/img/nodes/LightNodes.png)

轻节点具备以下行为：

1. 他们监听 ExtendedHeaders，即包装的“原始”区块头，通知 Celestia 节点新的区块头和相关的 DA 元数据。
2. 他们对接收到的区块头执行数据可用性采样 (DAS)

## 硬件要求

运行轻节点建议满足以下最低硬件要求：

* 内存：2 GB RAM
* CPU：单核
* 磁盘：5 GB SSD 存储
* 带宽：56 Kbps 下载/56 Kbps 上传

## 设置你的轻节点

本教程在 Ubuntu Linux 20.04 (LTS) x64 实例机器上执行。

### 设置依赖项

首先，确认更新和升级操作系统：

```sh
# 如果你使用的是 APT 包管理器
sudo apt update && sudo apt upgrade -y

# 如果你使用的是 YUM 软件包管理器
 sudo yum update
```

这些是执行多项任务（如下载文件、编译和监控节点）所必需的基本软件包：

```sh
# 如果你使用的是 APT 软件包管理器
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# 如果你使用的是 YUM 软件包管理器
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### 安装 Golang

Celestia-app 和 celestia-node 都是用 [Golang](https://go.dev/) 编写的，所以我们必须 安装 Golang 来构建和运行它们。

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

现在我们需要将 `/usr/local/go/bin` 目录添加到 `$PATH` 中：

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
跳转版本 go1.19.1 linux/amd64
```

### 安装 Celestia 节点

这里需要注意的是，你要决定哪个版本的celestia-node是你想要编译的。 Mamaki Testnet要求v0.3.0-rc2，Arabica Devnet要求 v0.3.0。

接下来的部分将着重于介绍如何为两个网络安装各自所需的版本。

#### Arabica Devnet安装

通过运行以下指令来安装celestia-node二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

验证二进制文件是否正常工作，并用celestia version命令检查版本

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

#### Mamaki Testnet 安装

通过运行以下指令来安装celestia-node二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

验证二进制文件是否正常工作，并用celestia version命令检查版本：

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## 初始化轻节点

运行以下指令：

```sh
celestia light init
```

指令运行后，你应该可以看到以下输出：

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### 开启轻节点

开启轻节点并连接到一个验证者节点的gRPC端点（通常在端口9090上显示)：

> 注意：为了获取得到/提交状态相关信息的能力，例如提交PayForData交易，或者查询节点账户余额，一个验证者（核心）节点的gPRC端点必须按照以下方式传递。

对于端口：

> 注意：这个 `--core.grpc.port` 默认为9090，所以如果你在命令行不具体指定端口，它将被默认设置为该端口。 你可以使用标准来指定你喜欢的另一个端口。

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

#### Arabica 设置

如果你需要连接到RPC端点列表，你可以查看这个列表 [这里](./arabica-devnet.md#rpc-endpoints)

例如，你的命令可能如下所示：

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

#### Mamaki 设置

如果你需要连接到RPC端点列表，你可以查看这个列表[这里](./mamaki-testnet.md#rpc-endpoints)

例如，你的命令可能如下所示：

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.remote https://rpc-mamaki.pops.one
```
<!-- markdownlint-enable MD013 -->

### 密钥和钱包

你可以通过运行以下命令为你的节点创建自己的密钥：

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

通过运行以下指令你可以用上面创建的密钥启动你的轻节点：

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

只要你启动了轻节点，将会为你生成一个钱包密钥。 你需要用测试网代币为该地址注资，以支付PayForData交易。

You can find the address by running the following command in the `celestia-node` directory:

```sh
./cel-key list --node.type light --keyring-backend test
```

You have two networks to get testnet tokens from:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a light node for your sovereign rollup, it is highly recommended to request Arabica devnet tokens as Arabica has the latest changes that can be used to test for developing your sovereign rollup. You can still use Mamaki Testnet as well, it is just used for Validator operations.

You can request funds to your wallet address using the following command in Discord:

```console
$request <Wallet-Address>
```

Where `<Wallet-Address>` is the `celestia1******` address generated when you created the wallet.

### Optional: run the light node with a custom key

In order to run a light node using a custom key:

1. The custom key must exist inside the celestia light node directory at the correct path (default: `~/.celestia-light/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <name_of_custom_key>
```
<!-- markdownlint-enable MD013 -->

### Optional: start light node with SystemD

Follow the tutorial on setting up the light node as a background process with SystemD [here](./systemd.md#celestia-light-node).

## Data availability sampling (DAS)

With your light node running, you can check out this tutorial on submitting `PayForData` transactions [here](../developers/node-tutorial.md).
