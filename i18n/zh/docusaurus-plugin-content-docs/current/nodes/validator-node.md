- - -
sidebar_label : 验证者节点
- - -

# 设置Celestia验证者节点

验证者节点允许您参与Celestia网络中的共识。

## 硬件要求

为运行验证者节点，推荐以下最低硬件配置：

* 内存: 8 GB RAM
* CPU：四核
* 硬盘： 250 GB SSD
* 带宽： 1 Gbps下载/100 Mbps上传

## 设置您的验证者节点

以下教程基于运行Ubuntu Linux 20.04 (LTS) x64的主机。

### 设置依赖项

请按照[这里](../developers/environment.md)的步骤安装依赖项

## 部署Celestia App

本节介绍Celestia验证者节点设置的第1部分： 运行内置Celestia Core节点的Celestia App服务。

> 注意：请确保您至少有100+ GB可用空间来安全安装+运行验证者节点。

### 安装Celestia App

请按照 [这里](../developers/celestia-app.md)的教程来安装Celestia App。

### 设置P2P网络

对于指南的这一部分，请选择您想要连接的网络：

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

之后，您可以继续本教程的其余部分。

### 配置剪枝参数

为降低硬盘空间的使用，我们建议按下面的参数来设置剪枝。 如果您想要： 根据情况，您可以将此更改为自己的剪枝参数：

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

### 配置验证者模式

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### 重置网络

这将删除所有数据文件夹，以便我们全新开始：

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### 可选：通过快照快速同步

根据您的硬件，从元初状态开始同步，可能需要很长时间。 使用场景 可以通过下载区块链的近期快照，来快速地同步您的Celestia节点。 如果你想要从元初状态同步，你可以跳过这一部分。

如果您要使用快照，请从下面的列表，选择您要同步的网络：

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### 通过SystemD启动Celestia-App

Follow the tutorial on setting up Celestia-App as a background process with SystemD [here](./systemd.md#start-the-celestia-app-with-systemd).

### Wallet

Follow the tutorial on creating a wallet [here](../developers/wallet.md).

### Delegate Stake to a Validator

Create an environment variable for the address:

```sh
VALIDATOR_WALLET=<validator-address>
```

If you want to delegate more stake to any validator, including your own you will need the `celesvaloper` address of the validator in question. You can either check it using the block explorer mentioned above or you can run the command below to get the `celesvaloper` of your local validator wallet in case you want to delegate more to it:

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

After entering the wallet passphrase you should see a similar output:

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

Next, select the network you want to use to delegate to a validator:

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Deploy the Celestia Node

This section describes part 2 of Celestia Validator Node setup: running a Celestia Bridge Node daemon.

### Install Celestia Node

You can follow the tutorial for installing Celestia Node [here](../developers/celestia-node.md)

### Initialize the Bridge Node

Run the following:

```sh
celestia bridge init --core.remote <ip:port of celestia-app> \
  --core.grpc http://<ip:port>
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

### Run the Bridge Node

Run the following:

```sh
celestia bridge start
```

### Optional: Start the Bridge Node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.

## Run a Validator Node

After completing all the necessary steps, you are now ready to run a validator! In order to create your validator on-chain, follow the instructions below. Keep in mind that these steps are necessary ONLY if you want to participate in the consensus.

Pick a `moniker` name of your choice! This is the validator name that will show up on public dashboards and explorers. `VALIDATOR_WALLET` must be the same you defined previously. Parameter `--min-self-delegation=1000000` defines the amount of tokens that are self delegated from your validator wallet.

Now, connect to the network of your choice.

You have the following option of connecting to list of networks shown below:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Complete the instructions in the respective network you want to validate in to complete the validator setup process.
