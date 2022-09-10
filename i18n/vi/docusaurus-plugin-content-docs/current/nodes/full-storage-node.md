- - -
sidebar_label : Full Storage Node
- - -

# Thiết lập node lưu trữ đầy đủ của Celestia

Tài liệu này sẽ hướng dẫn bạn cách thiết lập một Node lưu trữ đầy đủ của Celestia, là một node Celestia không kết nối với Ứng dụng Celestia (vì vậy không phải là một nút đầy đủ) nhưng lưu trữ tất cả dữ liệu.

## Yêu cầu phần cứng

Các yêu cầu tối thiểu về phần cứng sau đây được khuyến nghị để chạy node lưu trữ đầy đủ:

* Memory: 8 GB RAM
* CPU: Quad-Core
* Disk: 250 GB SSD Storage
* Bandwidth: 1 Gbps for Download/100 Mbps for Upload

## Thiết lập node lưu trữ đầy đủ của bạn

Hướng dẫn sau được thực hiện trên máy phiên bản Ubuntu Linux 20.04 (LTS) x64.

### Thiết lập các thành phần phụ thuộc

Bạn có thể làm theo hướng dẫn để thiết lập các phần phụ thuộc của mình [ here ](../developers/environment.md)

## Cài đặt Node Celestia

> Lưu ý: Đảm bảo rằng bạn có ít nhất 250 Gb dung lượng trống cho Node lưu trữ đầy đủ của Celestia

Bạn có thể làm theo hướng dẫn cài đặt Node Celestia [ here ](../developers/celestia-node.md)

### Chạy node lưu trữ đầy đủ

#### Khởi tạo node lưu trữ đầy đủ

Chạy lệnh sau:

```sh
celestia full init
```

#### Chạy node lưu trữ đầy đủ

Khởi động node lưu trữ đầy đủ với kết nối với điểm cuối gRPC của node validator (thường được hiển thị trên cổng 9090):

> LƯU Ý: Để có quyền truy cập vào khả năng nhận/ gửi liên quan đến tình trạng thông tin, chẳng hạn như khả năng gửi các giao dịch PayForData, hoặc truy vấn số dư tài khoản của node, điểm cuối gRPC của node validator (cốt lõi) phải được chuyển như hướng dẫn bên dưới.

```sh
celestia full start --core.grpc http://<ip addr of core node>:9090
```

Nếu bạn muốn tìm ví dụ các điểm cuối RPC, hãy xem danh sách tài nguyên [ here ](./mamaki-testnet.md#rpc-endpoints).

Bạn có thể tạo khóa cho node của mình bằng cách làm theo hướng dẫn ` cel-key ` [ here ](./keys.md)

Khi bạn khởi động Full Node, một khóa ví sẽ được xuất cho bạn. Bạn sẽ cần nạp tiền cho địa chỉ đó bằng mã thông báo Mamaki Testnet để thanh toán cho Giao dịch PayForData. Bạn có thể tìm địa chỉ bằng cách chạy lệnh sau:

```sh
./cel-key list --node.type full --keyring-backend test
```

Yêu cầu mã thông báo Mamaki Testnet tại [here](./mamaki-testnet.md#mamaki-testnet-faucet).

### Tùy chọn: chạy toàn bộ node lưu trữ bằng khóa tùy chỉnh

Để chạy một node lưu trữ đầy đủ bằng cách sử dụng khóa tùy chỉnh:

1. Khóa tùy chỉnh phải tồn tại trong thư mục node lưu trữ đầy đủ của celestia tại đúng đường dẫn (default: ` ~ /.celestia-full / keys / keyring-test `)
2. Tên của khóa tùy chỉnh phải được chuyển khi ` start `, như sau:

```sh
celestia full start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Tùy chọn: khởi động node lưu trữ đầy đủ với SystemD

Làm theo hướng dẫn về cách thiết lập node lưu trữ đầy đủ như quy trình xử lý nền với SystemD [ here ](./systemd.md#celestia-full-storage-node).

Như vậy, hiện tại bạn đang chạy Node lưu trữ đầy đủ của Celestia.
