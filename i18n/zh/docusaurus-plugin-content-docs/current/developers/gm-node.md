---
sidebar_label: 运行一个轻节点
---

# 🪶 运行一个 Celestia DA 轻节点

一个在 Mamaki 测试网上运行的Celestia轻节点需要完成这个教程。 运行以下的指令去安装Celestia-Node：

<!-- markdownlint-disable MD010 -->
```bash
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```
<!-- markdownlint-enable MD010 -->

![1.png](/img/gm/1.png)

在 celestia-节点 存储库内是一个名为 `cel-key`的工具，它使用了 Cosmos-SDK 在地平线下提供的 关键工具。 工具可以用于 `添加`,`删除`并管理任何 DA 节点的密钥 类型`(ridge || full || light)`, 或一般密钥。

## :old_key : 创建密钥

为节点创建密钥：

```bash
制作cel键
```

使用`celestiaversion`命令验证Celestia节点的版本， 它应该是＜code＞v0.3.0-rc2＜/code＞：

```bash
Celestia版本
```

```bash
#语义版本：v0.3.0-rc2
#提交：89892d8b96660e334741987d84546c36f0996fbe
#构建日期：2022年10月7日星期五01:08:14 UTC
#系统版本：amd64/linux
#Golang版本：go1.18.2
```

## 🟢 初始化轻节点

现在，我们准备初始化Celestia灯光节点。 您可以通过运行：

```bash
celestia light init
```

使用 `cel-key` 查询我们的密钥地址：

```bash
/cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 访问Faucet

在Celstia Discord中用 `#mamaki-faucet`频道去请求测试网代币：

```bash
$request <Wallet-Address>
```

用公共核心端点连接去启动Celestia轻节点

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![3.png](/img/gm/3.png)

在另一个终端窗中，从我们访问的faucet去检查余额：

```bash
curl -X GET http://localhost:26658/balance
```

你的回复看起来应该是这样的，以 JSON 格式表述 `utia`。

```bash
{"denom":"utia","amount":"100000000"}
```

现在我们设置安装了Go和 Ignite CLI，且我们的Celestia轻节点运行在我们的机器上，我们已经准备好建造、测试和启动我们的sovereign rollup。
