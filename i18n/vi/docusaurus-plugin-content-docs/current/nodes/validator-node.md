- - -
sidebar_label : Validator Node
- - -

# Thiết lập một Node Validator của Celestia

Các node validator cho phép bạn tham gia vào quá trình đồng thuận của mạng Celestia.

## Yêu cầu phần cứng

Các yêu cầu tối thiểu về phần cứng sau đây được khuyến nghị để chạy validator node:

* Memory: 8 GB RAM
* CPU: Quad-Core
* Disk: 250 GB SSD Storage
* Bandwidth: 1 Gbps đối với Download/100 Mbps đối với Upload

## Thiết lập validator node của bạn

Hướng dẫn sau được thực hiện trên phiên bản Ubuntu Linux 20.04 (LTS) x64.

### Thiết lập các thành phần phụ thuộc

Làm theo hướng dẫn tại đây để cài đặt các thành phần phụ thuộc [tại đây](../developers/environment.md).

## Triển khai celestia-app

Phần này mô tả phần 1 của việc thiết lập Validator Node của Celestia: chạy daemon của Celestia App với một node Celestia Core nội bộ.

> Lưu ý: Đảm bảo bạn có ít nhất 100 Gb dung lượng trống để cài đặt + chạy một cách an toàn     Validator Node.

### Cài đặt celestia-app

Làm theo hướng dẫn cài đặt Celestia App [tại đây](../developers/celestia-app.md).

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

### Định cấu hình chế độ validator

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### Reset mạng

Việc này sẽ xóa tất cả folder dữ liệu để chúng ta có thể bắt đầu lại từ đầu:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Tùy chọn: Sync nhanh với Snapshot

Đồng bộ hóa từ đầu có thể mất nhiều thời gian, tùy thuộc vào phần cứng của bạn. Sử dụng phương pháp này bạn có thể đồng bộ hóa node celestia của mình rất nhanh bằng cách tải xuống một snapshot gần đây của blockchain. Nếu bạn muốn sync từ đầu, bạn có thể bỏ qua phần này.

Nếu bạn muốn sử dụng snapshot, hãy xác định mạng bạn muốn đồng bộ hóa từ danh sách dưới đây:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Khởi động celestia-app với SystemD

Làm theo hướng dẫn về cách thiết lập Celestia-App làm trình chạy nền với SystemD [tại đây](./systemd.md#start-the-celestia-app-with-systemd).

### Ví

Làm theo hướng dẫn để tạo ví [tại đây](../developers/wallet.md).

### Ủy thác cho một validator

Tạo biến môi trường cho địa chỉ ví:

```sh
VALIDATOR_WALLET=<validator-address>
```

Nếu bạn muốn ủy thác thêm cổ phần cho bất kì validator nào, bao gồm của riêng bạn, bạn sẽ cần địa chỉ `celesvaloper` của validator trong phần câu hỏi. Bạn có thể kiếm tra nó bằng cách sử dụng block explorer được đề cập ở trên hoặc bạn có thể chạy lệnh bên dưới để nhận `celesvaloper` của ví validator của bạn trong trường hợp bạn muốn ủy thác thêm:

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

Sau khi gửi passphrase ví của bạn, bạn sẽ thấy kết quả tương tự như sau:

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

Kế tiếp, chọn mạng lưới bạn muốn ủy thác cho validator:

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Triển khai celestia-node

Phần này mô tả phần 2 của việc thiết lập Celestia Validator Node: chạy daemon của Bridge Node của Celestia.

### Cài đặt Node Celestia

Bạn có thể làm theo hướng dẫn sau để cài đặt Node Celestia [tại đây](../developers/celestia-node.md)

### Khởi tạo bridge node

Chạy lệnh sau:

```sh
celestia bridge init --core.ip <ip-address> --core.grpc.port <port>
```

> Lưu ý: `Cổng RPC chính` mặc định sẽ là 9090, nên nếu bạn không chọn một port cụ thể      trong câu lệnh, mặc định sẽ là port 9090. Bạn có thể sử dụng flag để chỉ định một port khác nếu bạn thích.

Nếu bạn cần danh sách các RPC endpoint để kết nối, bạn có thể kiểm tra từ danh sách [tại đây](./mamaki-testnet.md#rpc-endpoints)

### Chạy bridge node

Chạy lệnh sau:

```sh
celestia bridge start
```

### Tùy chọn: khởi chạy bridge node với SystemD

Làm theo hướng dẫn về cách thiết lập bridge node làm trình chạy nền với SystemD [tại đây](./systemd.md#celestia-bridge-node).

Bạn đã thiết lập thành công một bridge node đang đồng bộ với mạng.

## Chạy một Validator Node

Sau khi hoàn thành tất cả các bước, bây giờ bạn đã sẵn sàng chạy validator! Để bắt đầu khởi tạo validator on-chain, làm theo hướng dẫn sau. Hãy nhớ rằng tất cả các bước đều cần thiết nếu bạn muốn tham gia vào việc đồng thuận.

Chọn một tên `moniker` tùy ý của bạn! Đây là tên validator sẽ được hiển thị trên dashboard và explorers công khai. `VALIDATOR_WALLET` phải giống với địa chỉ ví bạn tạo trước đó. Thông số `--min-self-delegation=1000000` có nghĩa là số lượng token đang được tự ủy thác từ ví validator của bạn.

Bây giờ, kết nối với mạng lưới tùy bạn chọn.

Bạn có những lựa chọn sau đây để kết nối với mạng thể hiện bên dưới:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Hoàn thành hướng dẫn theo từng mạng lưới bạn muốn xác thực để hoàn thành quá trình thiết lập validator.
