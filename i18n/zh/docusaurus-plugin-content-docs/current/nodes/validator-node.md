- - -
sidebar_label : 验证者节点
- - -

# Setting up a Celestia Validator Node

验证者节点允许您参与Celestia网络中的共识。

## Hardware requirements

为运行验证者节点，推荐以下最低硬件配置：

* 内存: 8 GB RAM
* CPU：四核
* 硬盘： 250 GB SSD
* 带宽： 1 Gbps下载/100 Mbps上传

## Setting up your validator node

以下教程基于运行Ubuntu Linux 20.04 (LTS) x64的主机。

### Setup the dependencies

请按照[这里](../developers/environment.md)的步骤安装依赖项

## Deploying the celestia-app

本节介绍Celestia验证者节点设置的第1部分： 运行内置Celestia Core节点的Celestia App服务。

> 注意：请确保您至少有100+ GB可用空间来安全安装+运行验证者节点。

### Install celestia-app

请按照[这里](../developers/celestia-app.md)的教程来安装Celestia App。

### Setup the P2P networks

对于指南的这一部分，请选择您想要连接的网络：

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

之后，您可以继续本教程的其余部分。

### Configure pruning

For lower disk space usage we recommend setting up pruning using the configurations below. You can change this to your own pruning configurations if you want: 如果您想要： 根据情况，您可以将此更改为自己的剪枝参数：

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

### Configure validator mode

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### Reset network

这将删除所有数据文件夹，以便我们全新开始：

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Optional: quick-sync with snapshot

根据您的硬件，从元初状态开始同步，可能需要很长时间。 Syncing from Genesis can take a long time, depending on your hardware. Using this method you can synchronize your Celestia node very quickly by downloading a recent snapshot of the blockchain. If you would like to sync from the Genesis, then you can skip this part. 如果你想要从元初状态同步，你可以跳过这一部分。

如果您要使用快照，请从下面的列表，选择您要同步的网络：

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Start the celestia-app with SystemD

请按照[这里](./systemd.md#start-the-celestia-app-with-systemd)的教程，通过SystemD，将Celestia-App设置为后台进程。

### 钱包

请按照[这里](../developers/wallet.md)的教程来创建钱包。

### Delegate stake to a validator

为地址创建环境变量：

```sh
VALIDATOR_WALLET=<validator-address>
```

如果您想要将更多的抵押提供给验证者， 包括您自己的验证者，您需要验证者的`celesvaloper`地址。 If you want to delegate more stake to any validator, including your own you will need the `celesvaloper` address of the validator in question. You can either check it using the block explorer mentioned above or you can run the command below to get the `celesvaloper` of your local validator wallet in case you want to delegate more to it:

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

输入钱包密码后，你应该看到一个类似的输出：

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

接下来，选择您想要给验证者抵押的网络：

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Deploy the celestia-node

本节介绍Celestia验证者节点设置的第2部分：运行Celestia桥节点服务。

### Install celestia-node

请按照[这里](../developers/celestia-node.md)的教程来安装Celestia节点。

### Initialize the bridge node

运行以下命令：

```sh
celestia bridge init --core.remote tcp://<ip:port of celestia-app> \
  --core.grpc http://<ip:port>
```

如果您需要一个RPC端点列表来连接，您可以从邮件列表[中查看](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

运行以下命令：

```sh
celestia bridge start
```

### Optional: start the bridge node with SystemD

请按照[这里](./systemd.md#celestia-bridge-node)的教程，通过SystemD，将桥接节点设置为后台进程。

您已成功地设置了与网络同步的桥接节点。

## Run a validator node

After completing all the necessary steps, you are now ready to run a validator! In order to create your validator on-chain, follow the instructions below. Keep in mind that these steps are necessary ONLY if you want to participate in the consensus. 为了在链上创建您的验证者，请遵循下面的说明。 请记住，只有你想要参加共识时，这些步骤才是必要的。

选择一个`昵称`！ 这是在公共仪表板和浏览器中显示的验证者名称。 `VALIDATOR_WALLET` 必须与先前定义的相同。 Pick a `moniker` name of your choice! This is the validator name that will show up on public dashboards and explorers. `VALIDATOR_WALLET` must be the same you defined previously. Parameter `--min-self-delegation=1000000` defines the amount of tokens that are self delegated from your validator wallet.

现在，连接到您选择的网络。

您有以下网络列表可供选择：

* [Mamaki](./mamaki-testnet.md#connect-validator)

在您想要验证的网络中，执行相应（以上）步骤，以完成验证者设置过程。
