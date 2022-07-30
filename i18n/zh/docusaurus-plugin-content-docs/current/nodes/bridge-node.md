- - -
sidebar_label : 桥接节点
- - -

# 设置Celestia桥接节点

本教程将介绍如何设置 Celestia 桥接节点。

桥接节点连接数据可用性层和共识层，同时还可以选择成为验证者。

## 桥接节点概述

Celestia桥接节点具有以下属性：

1. 从共识网络中的受信任核心进程（意味着与 celestia-core 节点的受信RPC 连接）导入和处理“原始”区块头和块。 桥接节点可以在内部（嵌入式）运行此核心进程，也可以简单地连接到远程端点。 桥接节点还可以选择成为共识网络中的活跃验证者, 向 DA 网络中的轻节点提供带有数据可用性标头的块共享
2. 验证和擦除“原始”块的代码
3. 向 DA 网络中的轻节点提供带有数据可用性标头的块共享 ![桥接节点图](/img/nodes/BridgeNodes.png)

从实现的角度来看，桥接节点运行两个独立的进程：

1. 带有 Celestia Core 的 Celestia 应用程序（[参见 repo](https://github.com/celestiaorg/celestia-app)）

    * **Celestia App**是运行应用程序和权益证明逻辑的状态机。 Celestia App 基于 [Cosmos SDK](https://docs.cosmos.network/)构建，还包含**Celestia Core**。
    * **Celestia Core**是状态交互、共识和区块生产层。 Celestia Core 基于[Tendermint Core](https://docs.tendermint.com/)构建，经过修改以存储纠删码块的数据根以及其他更改（[请参阅 ADR](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)）。

2. Celestia 节点（[见 repo](https://github.com/celestiaorg/celestia-node)）

    * **Celestia Node**通过一个单独的 libp2p 网络对上述内容进行了扩充，该网络服务于数据可用性采样请求， 该团队有时将其称为“光环”网络。

## 硬件要求

建议使用以下硬件最低要求来运行桥接节点：

* 内存: 8 GB RAM
* CPU：四核
* 磁盘：250 GB SSD 存储
* 带宽： 1 Gbps下载/100 Mbps上传

## 设置您的桥接节点

以下教程是在 Ubuntu Linux 20.04 (LTS) x64 实例机器上完成的。

### 设置依赖项

请按照[这里](../developers/environment.md)的步骤安装依赖项

## 部署Celestia桥接节点

### 安装 Celestia 节点

安装 Celestia Node 二进制文件，它将用于运行 Bridge 节点。

请按照[这里](../developers/celestia-node.md)的教程来安装Celestia节点。

### 初始化桥接节点

运行以下命令：

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657 
```

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

### 运行桥接节点

启动桥接节点并连接到验证器节点的 gRPC 端点 (通常在端口 9090上显示)：

> 注意：为了获得获取/提交状态相关信息的能力，例如提交 PayForData 交易或查询节点账户余额的能力，验证者（核心）节点的 gRPC 端点必须按指示传递如下

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

如果您需要连接到的 RPC 端点列表，可以从[此处](./mamaki-testnet.md#rpc-endpoints)的列表中查看

您可以按照[这里](./keys.md)的`cel-key`指示步骤为您的节点创建密钥

启动桥接节点后，将为您生成一个钱包密钥。 您需要使用Mamaki 测试网代币来支付 PayForData 交易费用。 您可以通过运行以下命令找到地址：

```sh
./cel-key list --node.type bridge --keyring-backend test
```

可以在这里请求 [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)测试网代币

#### 可选：使用自定义密钥运行桥接节点

要使用自定义密钥运行桥接节点：

1. 自定义密钥必须存在于 celestia 桥接节点目录中的正确路径(默认: `~/.celestia-bridge/keys/keyring-test`)
2. 自定义密钥的名称必须在 `开始`时传递，就像这样：

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### 可选：通过SystemD启动桥接节点

请按照[这里](./systemd.md#celestia-bridge-node)的教程，通过SystemD，将桥接节点设置为后台进程。

您已成功地设置了与网络同步的桥接节点。
