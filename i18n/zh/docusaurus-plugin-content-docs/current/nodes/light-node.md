- - -
sidebar_label : 轻节点
- - -

# 设置 Celestia 轻节点

本教程将指导您设置 Celestia 轻节点，这将允许您在数据可用性 (DA) 网络上进行数据可用性采样。

> 点击[这里](../developers/light-node-video.md)查看设置 Celestia 轻节点的视频教程。

## 轻节点概述

轻节点确保数据可用性， 这是与 Celestia 网络交互的最常见方式。

![light-node](/img/nodes/LightNodes.png)

轻节点具有以下属性：

1. 它们监听 ExtendedHeaders，即包装的“原始”头，通知 Celestia 节点新的块头和相关的 DA 元数据。
2. 它们对接收到的标头执行数据可用性采样 (DAS)

## 硬件要求

建议运行轻节点满足以下最低硬件要求：

* 内存: 2 GB RAM
* CPU: 单核
* 磁盘：5 GB SSD 存储
* 带宽: 56 Kbps 下载/56 Kbps 上传

## 设置您的轻节点

以下教程是在 Ubuntu Linux 20.04 (LTS) x64 实例机器上完成的。

### 设置依赖项

首先，确保更新和升级操作系统：

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

这些是执行许多任务（如下载文件、编译和监控节点）所必需的基本安装包：

```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### 安装 Golang

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

### 安装 Celestia 节点

通过运行以下命令安装 celestia-node 二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

验证二进制文件是否正常工作并使用 celestia version 命令检查版本：

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## 初始化轻节点

运行以下命令：

```sh
celestia light init
```

运行命令后，可以看到以下输出：

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### 启动轻节点

启动轻节点并连接到验证器节点的 gRPC 端点 (通常在端口 9090上显示)：

> 注意：为了获得获取/提交状态相关信息的能力，例如提交 PayForData 交易或查询节点账户余额的能力，验证者（核心）节点的 gRPC 端点必须按指示传递如下

```sh
celestia light start --core.grpc http://<ip>:9090
```

如果您需要连接到 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

例如，您的命令可能如下所示：

```sh
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090
```

### 密钥和钱包

您可以通过运行以下命令为您的节点创建密钥：

```sh
make cel-key
```

启动轻节点后，将为您生成一个钱包密钥。 您需要使用 Mamaki 测试网代币为该地址注资，以支付 PayForData 交易。

您可以通过在 `celestia-node` 目录中运行以下命令来找到地址：

```sh
./cel-key list --node.type light --keyring-backend test
```

可以在[这里](./mamaki-testnet.md#mamaki-testnet-faucet)请求 Mamaki 测试网代币

### 可选：使用自定义密钥运行轻节点

要使用自定义密钥运行轻节点：

1. 自定义密钥必须存在于 celestia 轻节点目录中的正确路径(默认:`~/.celestia-light/keys/keyring-test`)
2. 自定义密钥的名称必须在 `开始`时传递，就像这样：

```sh
celestia light start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### 可选：通过 SystemD 启动轻节点

请按照[这里](./systemd.md#celestia-light-node)的教程，通过SystemD，将轻节点设置为后台进程。

## Data availability sampling (DAS)

With your light node running, you can check out this tutorial on submitting `PayForData` transactions [here](../developers/node-tutorial.md).
