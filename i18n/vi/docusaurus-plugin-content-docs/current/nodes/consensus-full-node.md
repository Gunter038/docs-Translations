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

Phần này mô tả phần 1 của việc thiết lập full node Consensus của Celestia: chạy daemon của Celestia App với một node Celestia Core nội bộ.

> Lưu ý: Đảm bảo bạn có ít nhất 100 Gb dung lượng trống để cài đặt + chạy một cách an toàn Full node Consensus.

### Cài đặt celestia-app

Làm theo hướng dẫn cài đặt Celestia Node [tại đây](../developers/celestia-app.md).

### Thiết lập mạng P2P

Đối với phần này của hướng dẫn, chọn mạng bạn muốn kết nối:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Sau đó, bạn có thể tiếp tục với phần còn lại của hướng dẫn.

### Configure pruning

Để sử dụng dung lượng ổ cứng thấp hơn, chúng tôi khuyên bạn nên thiết lập pruning bằng cách sử dụng configurations bên dưới. Bạn có thể thay đổi nó thành prunning configurations của riêng bạn nếu bạn muốn:

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

### Reset mạng

Việc này se xóa tất cả folder dữ liệu để chúng ta có thể bắt đầu lại từ đầu:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Tùy chọn: Sync nhanh với Snapshot

Đồng bộ hóa từ đầu có thể mất nhiều thời gian, tùy thuộc vào phần cứng của bạn. Sử dụng phương pháp này bạn có thể đồng bộ hóa node celestia của mình rất nhanh bằng cách tải xuống một snapshot gần đây của blockchain. Nếu bạn muốn sync từ đầu, bạn có thể bỏ qua phần này.

Nếu bạn muốn sử dụng snapshot, hãy xác định mạng bạn muốn đồng bộ hóa từ danh sách dưới đây:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Khởi chạy celestia-app

Để khởi chạy full node đồng thuận của bạn, chạy câu lệnh sau:

```sh
celestia-appd start
```

Câu lệnh này sẽ giúp bạn bắt đầu đồng bộ hóa với lịch sử blockchain Celestia.

### Tùy chọn: điều chỉnh RPC endpoint

Bạn có thể định cấu hình full node Consensus của mình thành một RPC endpoint công khai và lắng nghe bất kỳ kết nối nào từ các Data Availability Nodes để cung cấp các yêu cầu cho Data Availability API [tại đây](../developers/node-tutorial.md).

Lưu ý rằng bạn sẽ cần đảm bảo port 9090 được mở cho việc này.

Chạy câu lệnh sau:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

Khởi động lại `celestia-appd` trong bước trước để chạy được những configs đó.

### Khởi động celestia-app với SystemD

Làm theo hướng dẫn về cách thiết lập Celestia-App làm trình chạy nền với SystemD [tại đây](./systemd.md#start-the-celestia-app-with-systemd).
