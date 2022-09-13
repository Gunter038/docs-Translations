- - -
sidebar_label : 共识全节点
- - -

# 设置 Celestia 共识全节点
<!-- markdownlint-disable MD013 -->

共识全节点允许你在 Celestia 共识层同步区块链历史。

## 硬件要求:

运行共识全节点建议满足以下硬件最低要求：

* 内存: 8 GB RAM
* CPU：四核
* 磁盘：250 GB SSD 存储
* 带宽： 1 Gbps下载/100 Mbps上传

## 设置你的共识全节点

以下教程基于运行Ubuntu Linux 20.04 (LTS) x64的主机。

### 设置依赖项

请按照[这里](../developers/environment.md)的步骤安装依赖项

## 部署 Celestia App

本节介绍 Celestia 共识全节点设置的第 1 部分：使用内部 Celestia Core 节点运行 Celestia App 守护程序。

> 注意: 请确保您至少有 100+ Gb 的可用空间来安全安装 + 运行共识全节点。

### 安装 Celestia App

按照[这里](../developers/celestia-app.md)的教程进行安装 Celestia App 操作。

### 设置 P2P 网络

对于本指南的这一部分，请选择您要连接的网络：

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

之后，您可以继续本教程的其余部分。

### 配置修剪参数

为了降低磁盘空间使用率，我们建议您按照下面的配置来修剪。 如果需要，您可以将其更改为您自己的修剪配置：

```sh
PRUNING="custom"
PRUNING_KEEP_RECENT="100"
PRUNING_INTERVAL="10"

sed -i -e "s/^pruning *=.*/pruning = \"$PRUNING\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \
\"$PRUNING_KEEP_RECENT\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \
\"$PRUNING_INTERVAL\"/" $HOME/.celestia-app/config/app.toml
```

### 重置网络

这将删除所有数据文件夹，以便我们全新开始：

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### 可选：通过快照快速同步

从初始状态同步可能需要很长时间，具体取决于您的硬件。 使用这种方法，您可以通过下载区块链的最新快照来快速地同步您的 Celestia 节点。 如果您想从初始状态同步，则可以跳过此部分。

如果您要使用快照，请从下面的列表选择您要同步的网络：

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### 启动 celestia-app

为了启动您的共识全节点，请运行以下命令：

```sh
celestia-appd start
```

这将让您同步 Celestia 区块链历史。

### 可选：配置 RPC 端点

您可以将您的共识全节点配置为公共 RPC 端点并侦听来自数据可用性节点的任何连接，以便在[此处](../developers/node-tutorial.md)为数据可用性 API 的请求提供服务。

请注意，你需要确保 9090 端口是否对此开放。

运行以下命令:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

在上一步中重新启动 `celestia-appd` 来加载这些配置。

### Start the celestia-app with SystemD

Follow the tutorial on setting up Celestia-App as a background process with SystemD [here](./systemd.md#start-the-celestia-app-with-systemd).
