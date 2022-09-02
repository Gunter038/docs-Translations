- - -
sidebar_label : Config.toml 指南
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

本节是Celestia桥接节点所需要的。 默认情况下，`Remote = false`. 同样对于开发网，我们需要使用远程核心选项，这也可以通过命令行标志`--core.remote`实现.

### [P2P]

#### 启动设置

Bootstrapers 帮助新节点更快地在网络中找到对等点。 默认情况下， `Bootstraper = fals` 和 `BootstrapPeers` 是空的。 如果您希望您的节点成为一个Bootstraper，请激活 `Bootstraper = true`。 `BootstrapPeers`在初始化期间已经默认提供。 如果您想要手动添加您自己，您需要提供 对等点的多地址。

#### 对等节点

此配置的目的是建立双向通信。 Celestia 桥节接点通常就是这种情况。 此外，您 需要将字段 `PeerExchange` 从false改为true。

### [服务]

#### 受信任哈希和受信任节点

当使用已经在运行的`远程`Celestia核心节点时，需要`受信任哈希`才能正确初始化Celestia桥接节点。 如果在初始化阶段没有手动提供哈希，Celestia轻节点将使用元初哈希作为受信任的哈希。

`受信任节点`是Celestia轻节点信任的桥节点的数组。 默认情况下，如果用户没有在配置文件中设置受信任节点，启动用对等节点会被Celestia轻节点当作受信任节点。

任何Celestia桥接结点都可以用作轻节点的受信任节点。 不过，按照设计，轻节点不能作为其它轻节点的受信任节点。
