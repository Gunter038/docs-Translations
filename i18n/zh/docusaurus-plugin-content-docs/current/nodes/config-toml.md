- - -
sidebar_labar: Config.toml 指南
- - -

# Config.toml 分类

- [Config.toml 分类](#configtoml-breakdown)
  - [前置条件](#pre-requisites)
  - [了解 config.toml](#understanding-configtoml)
    - [[Core]](#core)
    - [[P2P]](#p2p)
      - [Bootstrap版本](#bootstrap)
      - [相互对等点](#mutual-peers)
    - [[服务]](#services)
      - [信任哈希和信任点](#trustedhash-and-trustedpeer)

## 前置条件

请确保您已经安装并初始化celestia节点

## 了解 config.toml

初始化后，对于任何类型的节点，您将在以下路径中找到一个 `config.toml` (默认位置)：

- `$HOME/.celestia-bridge/config.toml` for bridge node
- `$HOME/.celestia-light/config.toml` for light node

让我们拆解一些最常用的章节。

### [Core]

本节是Celestia桥接节点所需要的。 默认情况下，`Remote = false`. 仍然对于 devnet，我们要去使用远程核心选项，这也可以设置通过命令行标志`--core.remote`.

### [P2P]

#### Bootstrap

Bootstrapers 帮助新节点更快地在网络中找到对等点。 默认情况下， `Bootstraper = fals` 和 `BootstrapPeers` 是空的。 如果您希望您的节点成为一个Bootstraper，请激活 `Bootstraper = true`。 `BootstrapPeers`在初始化期间已经默认提供。 如果您想要手动添加您自己，您需要提供 对等点的多地址。

#### 相互对等点

此配置的目的是建立双向通信。 Celestia 桥节接点通常就是这种情况。 此外，您 需要将字段 `PeerExchange` 从false改为true。

### [服务]

#### 信任哈希和信任点

`TrustedHash` is needed to properly initialize a Celestia Bridge Node with an already-running `Remote` Celestia Core node. Celestia Light Node will take a genesis hash as the trusted one, if no hash is manually provided during initialization phase.

`TrustedPeers` is the array of Bridge Nodes' peers that Celestia Light Node trusts. By default, bootstrap peers becomes trusted peers for Celestia Light Nodes if a user is not setting the trusted peer params in config file.

Any Celestia Bridge Node can be a trusted peer for the Light one. However, the Light node by design can not be a trusted peer for another Light Node.
