- - -
sidebar_label : Bridge Node
- - -

# Setting up a Celestia Bridge Node

This tutorial will go over the steps to setting up your Celestia Bridge node.

Bridge nodes connect the data availability layer and the consensus layer while also having the option of becoming a validator.

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
celestia bridge init --core.remote tcp://<ip-address>:26657
```

Nếu bạn cần danh sách các endpoints RPC để kết nối, bạn có thể kiểm tra từ danh sách [ here ](./mamaki-testnet.md#rpc-endpoints)

### Chạy bridge node

Khởi động Bridge Node bằng kết nối với endpoint của node's gRPC validator (thường được hiển thị trên cổng 9090):

> LƯU Ý: Để có quyền truy cập vào khả năng nhận / gửi thông tin liên quan đến trạng thái, chẳng hạn như khả năng gửi các giao dịch PayForData hoặc truy vấn số dư tài khoản các nodes, một endpoint gRPC của validator (core) phải được chuyển như chỉ dẫn bên dưới._

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

Nếu bạn cần danh sách các endponit RPC để kết nối, bạn có thể kiểm tra từ danh sách [ here ](./mamaki-testnet.md#rpc-endpoints)

You can create your key for your node by following the `cel-key` instructions [here](./keys.md)

Once you start the Bridge Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Mamaki Testnet tokens can be requested [here](./mamaki-testnet.md#mamaki-testnet-faucet).

#### Optional: run the bridge node with a custom key

In order to run a bridge node using a custom key:

1. The custom key must exist inside the celestia bridge node directory at the correct path (default: `~/.celestia-bridge/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: start the bridge node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.
