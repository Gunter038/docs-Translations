- - -
sidebar_label : 共识全节点
- - -

# Setting up a Celestia Consensus Full Node
<!-- markdownlint-disable MD013 -->

共识全节点允许你在 Celestia 共识层同步区块链历史。

## Hardware requirements

为运行验证者节点，推荐以下最低硬件配置：

* 内存: 8 GB RAM
* CPU：四核
* 磁盘：250 GB SSD 存储
* 带宽： 1 Gbps下载/100 Mbps上传

## Setting up your consensus full node

以下教程基于运行Ubuntu Linux 20.04 (LTS) x64的主机。

### Setup the dependencies

请按照[这里](../developers/environment.md)的步骤安装依赖项

## Deploying the celestia-app

This section describes part 1 of Celestia consensus full node setup: running a Celestia App daemon with an internal Celestia Core node.

> Note: Make sure you have at least 100+ Gb of free space to safely install + run the consensus full node.

### Install celestia-app

Follow the tutorial on installing Celestia App [here](../developers/celestia-app.md).

### Setup the P2P networks

For this section of the guide, select the network you want to connect to:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

After that, you can proceed with the rest of the tutorial.

### Configure pruning

For lower disk space usage we recommend setting up pruning using the configurations below. You can change this to your own pruning configurations if you want: You can change this to your own pruning configurations if you want:

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

### Reset network

This will delete all data folders so we can start fresh:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Optional: quick-sync with snapshot

Syncing from Genesis can take a long time, depending on your hardware. Syncing from Genesis can take a long time, depending on your hardware. Using this method you can synchronize your Celestia node very quickly by downloading a recent snapshot of the blockchain. If you would like to sync from the Genesis, then you can skip this part. If you would like to sync from the Genesis, then you can skip this part.

If you want to use snapshot, determine the network you would like to sync to from the list below:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Start the celestia-app

In order to start your consensus full node, run the following:

```sh
celestia-appd start
```

This will let you sync the Celestia blockchain history.

### Optional: configure for RPC endpoint

You can configure your Consensus Full Node to be a public RPC endpoint and listen to any connections from Data Availability Nodes in order to serve requests for the Data Availability API [here](../developers/node-tutorial.md).

Note that you would need to ensure port 9090 is open for this.

Run the following commands:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

Restart `celestia-appd` in the previous step to load those configs.

### Start the celestia-app with SystemD

Follow the tutorial on setting up Celestia-App as a background process with SystemD [here](./systemd.md#start-the-celestia-app-with-systemd).
