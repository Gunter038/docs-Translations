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

One thing to note here is deciding which version of celestia-node you wish to compile. Mamaki Testnet requires v0.3.0-rc2 and Arabica Devnet requires v0.3.0.

The following sections highlight how to install it for the two networks.

#### Arabica Devnet installation

Install the celestia-node binary by running the following commands:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

#### Mamaki Testnet installation

Install the celestia-node binary by running the following commands:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Initialize the light node

Run the following command:

```sh
celestia light init
```

You should see output like:

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### Start the light node

Start the light node with a connection to a validator node's gRPC endpoint (which is usually exposed on port 9090):

> NOTE: In order for access to the ability to get/submit state-related information, such as the ability to submit PayForData transactions, or query for the node's account balance, a gRPC endpoint of a validator (core) node must be passed as directed below.

For ports:

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

#### Arabica Setup

If you need a list of RPC endpoints to connect to, you can check from the list [here](./arabica-devnet.md#rpc-endpoints)

For example, your command might look something like this:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

#### Mamaki Setup

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

For example, your command might look something like this:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.remote https://rpc-mamaki.pops.one
```
<!-- markdownlint-enable MD013 -->

### Keys and wallets

You can create your key for your node by running the following command:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

You can start your light node with the key created above by running the following command:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

Once you start the Light Node, a wallet key will be generated for you. You will need to fund that address with testnet tokens to pay for PayForData transactions.

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
