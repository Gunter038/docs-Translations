- - -
sidebar_label : Bridge Node
- - -

# Thiết lập một Node Bridge của Celestia

Hướng dẫn này sẽ trình bày các bước để thiết lập node Celestia Bridge của bạn.

Các node Bridge kết nối lớp Data Availability và lớp Đồng Thuận trong khi đồng thời đưa ra lựa chọn để trở thành một validator.

Validators không phải chạy các nodes cầu nối, nhưng được khuyến khích để để chuyển tiếp các khối tới mạng dữ liệu khả dụng.

## Tổng quan về các bridge nodes

Một node Celestia bridge có các thuộc tính sau:

1. Nhập và xử lý tiêu đề "raw" & amp; blocks từ một quy trình Core tin cậy (có nghĩa là một kết nối RPC đáng tin cậy với một nút lõi celestia) trong mạng đồng thuận. Các nodes bridge có thể chạy quy trình cốt lõi này  nội bộ (embedded) hoặc chỉ cần kết nối với một điểm cuối từ xa. Bridge Nodes cũng có tùy chọn trở thành valodators tích cực trong mạng đồng thuận.
2. Xác thực và xóa code các khối "raw"
3. Supply blocks chia sẻ với các headers về tính khả dụng của dữ liệu với các Nodes Light trong mạng DA.![bridge-node-diagram](/img/nodes/BridgeNodes.png)

Từ góc độ triển khai, các Bridge Nodes chạy hai quy trình riêng biệt:

1. Celestia App with Celestia Core ([see repo](https://github.com/celestiaorg/celestia-app))

    * ** Ứng dụng Celestia ** là trạng thái máy mà ứng dụng và proof-of-stake logic được chạy. Celestia App is built on [Cosmos SDK](https://docs.cosmos.network/) and also encompasses **Celestia Core**.
    * ** Celestia Core ** là layer tương tác trạng thái, đồng thuận và tạo ra khối. Celestia Core được xây dựng trên [ Tendermint Core ](https://docs.tendermint.com/), được sửa đổi để lưu trữ data roots của các khối được mã hóa xóa trong các thay đổi khác ([ see ADRs ](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia Node ([see repo](https://github.com/celestiaorg/celestia-node))

    * ** Celestia Node ** tăng cường phần trên với một mạng libp2p riêng biệt phục vụ các yêu cầu lấy mẫu về tính khả dụng của dữ liệu. Team đôi khi xem nó như là mạng "halo".

## Yêu cầu phần cứng

Các yêu cầu tối thiểu về phần cứng sau đây được khuyến nghị để chạy bridge node:

* Bộ nhớ: 8 GB RAM
* CPU: Quad-Core
* Ổ đĩa: 250 GB SSD Storage
* Bandwidth: 1 Gbps for Download/100 Mbps for Upload

## Thiết lập bridge node của bạn

Hướng dẫn sau được thực hiện trên máy tính phiên bản Ubuntu Linux 20.04 (LTS) x64.

### Thiết lập các phụ thuộc

Làm theo hướng dẫn tại đây để cài đặt các phần phụ thuộc [ here ](../developers/environment.md).

## Triển khai Celestia bridge node

### Cài đặt Node Celestia

Cài đặt binary Celestia Node, chúng sẽ được sử dụng để chạy Bridge Node.

Làm theo hướng dẫn cài đặt Celestia Node [ here ](../developers/celestia-node.md).

### Khởi tạo bridge node

Chạy lệnh sau:

```sh
celestia bridge init --core.ip <ip-address> --core.rpc.port <port>
```

> Lưu ý: `Cổng RPC chính` mặc định sẽ là 26657, nên nếu bạn không chọn một port cụ thể      trong câu lệnh, mặc định sẽ là port 26657. Bạn có thể sử dụng flag để chỉ định một port khác nếu bạn thích.

Nếu bạn cần danh sách các endpoints RPC để kết nối, bạn có thể kiểm tra từ danh sách [ here ](./mamaki-testnet.md#rpc-endpoints)

### Chạy bridge node

Khởi động Bridge Node bằng kết nối với endpoint của node's gRPC validator (thường được hiển thị trên cổng 9090):

```sh
celestia bridge start --core.ip <ip-address> --core.grpc.port <port>
```

> Lưu ý: `Cổng RPC chính` mặc định sẽ là 9090, nên nếu bạn không chọn một port cụ thể      trong câu lệnh, mặc định sẽ là port 9090. Bạn có thể sử dụng flag để chỉ định một port khác nếu bạn thích.

Nếu bạn cần danh sách các endponit RPC để kết nối, bạn có thể kiểm tra từ danh sách [ here ](./mamaki-testnet.md#rpc-endpoints)

Bạn có thể tạo khóa cho node của mình bằng cách làm theo hướng dẫn ` cel-key ` [ here ](./keys.md)

Khi bạn khởi động Bridge Node, một mã khóa ví sẽ được tạo. Bạn cần phải nạp tiền cho địa chỉ ví đó với token Testnet để trả gas cho giao dịch PayForData. Bạn có thể tìm thấy địa chỉ ví bằng cách chạy lệnh sau:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Bạn có thể nhận tokens testnet từ 2 mạng:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> Lưu ý: Nếu bạn đang chạy node bridge cho validator của bạn, bạn nên yêu cầu testnet tokens Mamaki     bởi vì đây là mạng testnet được sử dụng để thử nghiệm việc vận hành validator.

#### Tùy chọn: chạy bridge node bằng khóa tùy chỉnh

Để chạy một bridge node bằng cách sử dụng khóa tùy chỉnh:

1. Khóa tùy chỉnh phải tồn tại trong thư mục celestia bridge node tại đúng đường dẫn (default: ` ~ /.celestia-bridge / keys / keyring-test `)
2. Tên của khóa tùy chỉnh phải được chuyển theo ` start `, như sau:

```sh
celestia bridge start --core.ip <ip> --core.grpc.port 9090 --keyring.accname <name_of_custom_key>
```

### Tùy chọn: bắt đầu bridge node với SystemD

Theo hướng dẫn về cách thiết lập bridge node làm quy trình nền với SystemD [ here ](./systemd.md#celestia-bridge-node).

Bạn đã thiết lập thành công một bridge node đồng bộ với mạng.
