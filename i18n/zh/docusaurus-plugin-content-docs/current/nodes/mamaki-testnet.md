- - -
sidebar_label : Mamaki测试网
- - -

# Mamaki测试网
<!-- markdownlint-disable MD013 -->

![mamaki-testnet](/img/mamaki.png)

本指南包含如何连接到Mamaki的相关内容，取决于您正在运行的节点类型。 Mamaki是Celestia的一个里程碑，它使得任何人都可以在网络中测试核心的功能。 公告在[这里](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/)。

您参与的最佳方式取决于首先确定的您想运行的节点类型。 每种节点的指南都将链接到相关的网络，以便向您展示如何连接它们。

关于您可以运行的节点类型，就Mamaki而言，选项列表为：

共识

* [验证者节点](./validator-node.md)
* [参与共识的全节点](./consensus-full-node.md)

数据可用性

* [桥接节点](./bridge-node.md)
* [存储全节点](./full-storage-node.md)
* [轻节点](./light-node.md)

选择你想运行的节点类型，然后按照各个对应颜面的指示操作。 每当这些指南询问你想连接的网络时，请选择这些页面上的`Mamaki`相关内容，以便连接到Mamki。

## RPC终结点

可用于连接Mamaki测试网的RPC终结点列表：

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://rpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)

## Mamki测试网水龙头

> 这个水龙头与主网上的Celestia代币空投没有任何关系。 主网上的Celestia代码目前还不存在，也没有任何公开销售或分配的计划。

您可以通过Celestia的Discord服务器上的#faucet频道，向Mamaki测试网水龙头发出以下申请：

```text
$request <CELESTIA-ADDRESS> 
```

`<CELESTIA-ADDRESS>`是一个形如`celestia1****`的地址。

> 注意：每个地址/Discord ID，每周最多10个代币

## 浏览器

有几个可用于Mamaki的浏览器：

* [https://celestia.explorers.guru/](https://celestia.explorers.guru/)
* [https://celestiascan.vercel.app/](https://celestiascan.vercel.app/)

## 设置P2P网络

现在我们将通过克隆网上的代码库来设置P2P网络：

```sh
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git
```

要初始化网络，请选择一个描述您的节点的“节点名称”。 我们在此使用的--chain-id参数是 `mamaki`。 请注意，如果部署了新的测试网，这可能会改变。

```sh
celestia-appd init "node-name" --chain-id mamaki
```

复制`genesis.json`文件。 对于Mamaki，我们使用：

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

设置种子和对等节点：

```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml

```

注意：您可以在[这里](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt)找到更多对等节点。

您可以点击

这里<0>来返回离开桥接节点指南时的位置</p> 



## 通过快照来快速同步

运行下面的命令，从快照快速同步`mamaki`：



```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```


您可以点击这里<0>来返回离开桥接节点指南时的位置</p> 



## 向验证者提供抵押

要将代币委派给`celestiavaloper`验证者，作为示例，您可以运行：



```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```


如果成功，您应该看到类似的输出为：



```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```


您可以通过在区块浏览器中，输入返回的`txhash` ID，来检查TX是否执行顺利。

您可以点击这里<0>来返回离开桥接节点指南时的位置</p> 



## 连接验证

继续验证者教程，这是将您的验证者连接到Mamaki的步骤：



```sh
MONIKER="your_moniker"
VALIDATOR_WALLET="validator"

celestia-appd tx staking create-validator \
    --amount=1000000utia \
    --pubkey=$(celestia-appd tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=mamaki \
    --commission-rate=0.1 \
    --commission-max-rate=0.2 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1000000 \
    --from=$VALIDATOR_WALLET \
    --keyring-backend=test
```


您将被提示确认该交易：



```console
confirm transaction before signing and broadcasting [y/N]: y
```


输入`y`应该出现类似于以下输出：



```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```


您现在应该能够从取块浏览器中，看到您的验证者，就像[在这里](https://celestia.explorers.guru/)
