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

Một lưu ý đối với ports:

> Lưu ý: `Cổng RPC chính` mặc định sẽ là 9090, nên nếu bạn không chọn một port cụ thể      trong câu lệnh, mặc định sẽ là port 26657. Bạn có thể sử dụng flag để chỉ định một port khác nếu bạn thích.

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port>
```
<!-- markdownlint-enable MD013 -->

Nếu bạn muốn tìm ví dụ các điểm cuối RPC, hãy xem danh sách tài nguyên [ here ](./mamaki-testnet.md#rpc-endpoints).

Bạn có thể tạo khóa cho node của mình bằng cách làm theo hướng dẫn ` cel-key ` [ here ](./keys.md)

Khi bạn khởi động Full Node, một khóa ví sẽ được xuất cho bạn. Bạn cần phải nạp tiền cho địa chỉ ví đó với token Testnet để trả gas cho giao dịch PayForData. Bạn có thể tìm địa chỉ bằng cách chạy lệnh sau:

```sh
./cel-key list --node.type full --keyring-backend test
```

Bạn có thể nhận tokens testnet từ 2 mạng:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> Lưu ý: Nếu bạn đang chạy một full-storage node cho     rollup chuyên dụng của bạn, bạn nên yêu cầu Arabica devnet tokens     vì Arabica có những thay đổi và cập nhật mới nhất có thể được sử dụng để thử nghiệm cho việc phát triển rollup chuyên dụng của bạn. Bạn vẫn có thể sử dụng    Testnet Mamaki. Nó được sử dụng cho việc vận hành Validator.

### Tùy chọn: chạy toàn bộ node lưu trữ bằng khóa tùy chỉnh

Để chạy một node lưu trữ đầy đủ bằng cách sử dụng khóa tùy chỉnh:

1. Khóa tùy chỉnh phải tồn tại trong thư mục node lưu trữ đầy đủ của celestia tại đúng đường dẫn (default: ` ~ /.celestia-full / keys / keyring-test `)
2. Tên của khóa tùy chỉnh phải được chuyển khi ` start `, như sau:

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port> --keyring.accname <name-of-custom-key>
```
<!-- markdownlint-enable MD013 -->

### Tùy chọn: khởi động node lưu trữ đầy đủ với SystemD

Làm theo hướng dẫn về cách thiết lập node lưu trữ đầy đủ như quy trình xử lý nền với SystemD [ here ](./systemd.md#celestia-full-storage-node).

Như vậy, hiện tại bạn đang chạy Node lưu trữ đầy đủ của Celestia.
