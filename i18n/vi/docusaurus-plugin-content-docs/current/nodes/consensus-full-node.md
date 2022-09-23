- - -
sidebar_label : Consensus Full Node
- - -

# Thiết lập node đồng thuận Celestia đầy đủ
<!-- markdownlint-disable MD013 -->

Các nodes đồng thuận đầy đủ cho phép bạn đồng bộ hóa lịch sử blockchain trong lớp đồng thuận của Celestia.

## Yêu cầu phần cứng

Các yêu cầu tối thiểu về phần cứng được khuyến nghị để chạy Consensus full node:

* Memory: 8 GB RAM
* CPU: Quad-Core
* Disk: 250 GB SSD Storage
* Bandwidth: 1 Gbps đối với Download/100 Mbps đối với Upload

## Thiết lập Consensus full node

Hướng dẫn sau được thực hiện trên phiên bản Ubuntu Linux 20.04 (LTS) x64.

### Thiết lập các thành phần phụ thuộc

Làm theo hướng dẫn tại đây để cài đặt các thành phần phụ thuộc [tại đây](../developers/environment.md).

## Triển khai celestia-app

This section describes part 1 of Celestia consensus full node setup: running a Celestia App daemon with an internal Celestia Core node.

> Note: Make sure you have at least 100+ Gb of free space to safely install + run the consensus full node.

### Cài đặt celestia-app

Follow the tutorial on installing Celestia App [here](../developers/celestia-app.md).

### Thiết lập mạng P2P

For this section of the guide, select the network you want to connect to:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Sau đó, bạn có thể tiếp tục với phần còn lại của hướng dẫn.

### Configure pruning

For lower disk space usage we recommend setting up pruning using the configurations below. You can change this to your own pruning configurations if you want:

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

Syncing from Genesis can take a long time, depending on your hardware. Using this method you can synchronize your Celestia node very quickly by downloading a recent snapshot of the blockchain. If you would like to sync from the Genesis, then you can skip this part.

If you want to use snapshot, determine the network you would like to sync to from the list below:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Khởi chạy celestia-app

Để khởi chạy full node đồng thuận của bạn, chạy câu lệnh sau:

```sh
celestia-appd start
```

Câu lệnh này sẽ giúp bạn bắt đầu đồng bộ hóa với lịch sử blockchain Celestia.

### Tùy chọn: điều chỉnh RPC endpoint

You can configure your Consensus Full Node to be a public RPC endpoint and listen to any connections from Data Availability Nodes in order to serve requests for the Data Availability API [here](../developers/node-tutorial.md).

Note that you would need to ensure port 9090 is open for this.

Chạy câu lệnh sau:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

Khởi động lại `celestia-appd` trong bước trước để chạy được những configs đó.

### Start the celestia-app with SystemD

Follow the tutorial on setting up Celestia-App as a background process with SystemD [here](./systemd.md#start-the-celestia-app-with-systemd).
