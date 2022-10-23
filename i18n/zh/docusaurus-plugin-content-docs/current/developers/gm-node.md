---
sidebar_label: 运行一个轻节点
---

# 🪶 运行一个 Celestia DA 轻节点

该教程将指引你搭建一个在 Mamaki 测试网上运行的Celestia轻节点。 运行以下的指令来安装Celestia-Node：

<!-- markdownlint-disable MD010 -->
```bash
cd $HOME && rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node
git checkout tags/v0.3.0-rc2
make install
```
<!-- markdownlint-enable MD010 -->

![1.png](/img/gm/1.png)

在 celestia-node 存储库中有一个名为的工具`cel-key`，它在后台使用 Cosmos-SDK 提供的密钥。 此工具可以用于`添加`、`删除`和管理任何DA节点的密钥 输入 `(bridge || full || light)`，或者只是一般的密钥键。

## 🗝 创建一个密钥

为节点创建密钥：

```bash
创建 cel-key
```

使用`celestia version`命令验证Celestia节点的版本， 它应该是`v0.3.0-rc2`：

```bash
Celestia版本
```

```bash
# 输出

#Semantic version: v0.3.0-rc2
#Commit: 89892d8b96660e334741987d84546c36f0996fbe
#Build Date: Fri Oct  7 01:08:14 UTC 2022
#System version: amd64/linux
#Golang version: go1.18.2
```

## 🟢 初始化轻节点

现在，我们准备初始化Celestia轻节点。 您可以运行：

```bash
celestia light init
```

使用 `cel-key` 查询我们的密钥地址：

```bash
./cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 🚰 访问Faucet

进入Celestia的Discord `#mamaki-faucet`频道去获取测试网代币：

```bash
$request <Wallet-Address>
```

使用公共的端点启动一个Celestia轻节点

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![3.png](/img/gm/3.png)

在另一个终端窗中，检查我们在faucet获得的余额：

```bash
curl -X GET http://localhost:26658/balance
```

你的请求结果应该是这样，以 JSON 格式展示 `utia`。

```bash
{"denom":"utia","amount":"100000000"}
```

现在我们设置安装了Go和 Ignite CLI，且我们的Celestia轻节点运行在我们的机器上，我们已经准备好建造、测试和启动我们的sovereign rollup。
