---
sidebar_label: Arabica开发网
---

# Arabica开发网
<!-- markdownlint-disable MD013 -->

![arabica开发网](/img/arabica-devnet.png)

Arabia 开发网是一个来自Celestia Labs的新测试网，它完全侧重于 为开发者提供更强的性能和最新的升级来测试开发者的Rollups和应用。

Arabica并不注重验证者或共识级别的测试，而这是 Mamaki 测试网的作用。 如果你是一个验证者，我们建议只在 Mamaki [（这里）](./mamaki-testnet.md)上测试您的验证器操作

由于Arabica已经从Celestia的所有产品中安装了最新的版本，它可能会发生许多变化。 因此，作为一个公平警告，Arabica可能出乎意料地崩溃，但如果它被连续不断地更新，它是不断测试软件最新变化的一种有效方式。

开发者仍然可以在Mamaki测试网上部署他们的sovereign rollups。如果他们选择使用Mamaki 测试网，Mamaki测试网总是会落后于Arabica开发网，直到Mamaki测试网与验证者协调进行硬分叉升级。

## 集成

本指南包含如何连接到Mamaki的相关内容，取决于您正在运行的节点类型。

你最好的参与方式是首先确定你想运行的节点。 每个节点指南将链接到相关的网络，以便向你展示如何连接它们。

你可以通过运行以下节点类型来参与Arabica开发网：

数据可用性

* [桥接节点](./bridge-node.md)
* [存储全节点](./full-storage-node.md)
* [轻节点](./light-node.md)

选择你想运行的节点类型，然后按照各个相应页面的指示操作。 每当你被要求选择网络类型时，你需要在这些指引下进行连接，选择 `Arabica` 以参考页面上有关如何连接到Arabica的正确说明。

## RPC端点

可用于连接Arabica开发网的RPC端点列表：

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Arabica开发网水龙头

> 使用这个测试水龙头不会授予你任何空投或主网 CELESTIA 代币的分发。 目前还没有Celestia主网代币，也没有任何公开销售或分发的计划

您可以通过Celestia的Discord服务器上的#faucet频道，向Arabica开发网水龙头发出以下申请：

```text
$request <CELESTIA-ADDRESS>
```

`<CELESTIA-ADDRESS>`是一个形如`celestia1****`的地址。

> 注意：每个地址/Discord ID，每周最多领取10个代币

## 浏览器

这是一个你可以用于Arabica开发网的浏览器：

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
