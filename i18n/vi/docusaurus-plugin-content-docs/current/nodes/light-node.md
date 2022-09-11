- - -
sidebar_label : Light Node
- - -

# Setting up a Celestia Light Node

This tutorial will guide you through setting up a Celestia light node, which will allow you to perform data availability sampling on the data availability (DA) network.

> To view a video tutorial for setting up a Celestia light node, click [here](../developers/light-node-video.md)

## Overview of light nodes

Light nodes ensure data availability. This is the most common way to interact with the Celestia network.

![light-node](/img/nodes/LightNodes.png)

Light nodes have the following behavior:

1. They listen for ExtendedHeaders, i.e. wrapped “raw” headers, that notify Celestia nodes of new block headers and relevant DA metadata.
2. They perform data availability sampling (DAS) on the received headers

## Hardware requirements

The following minimum hardware requirements are recommended for running a light node:

* Memory: 2 GB RAM
* CPU: Single Core
* Disk: 5 GB SSD Storage
* Bandwidth: 56 Kbps for Download/56 Kbps for Upload

## Setting up your light node

This tutorial was performed on an Ubuntu Linux 20.04 (LTS) x64 instance machine.

### Setup the dependencies

First, make sure to update and upgrade the OS:

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

These are essential packages that are necessary to execute many tasks like downloading files, compiling, and monitoring the node:

```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### Install Golang

Celestia-app and celestia-node are written in [Golang](https://go.dev/) so we must install Golang to build and run them.

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Now we need to add the `/usr/local/go/bin` directory to `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Để kiểm tra xem Go đã được cài đặt để chạy đúng chưa:

```sh
phiên bản
```

Đầu ra phải là phiên bản được cài đặt:

```sh
vào phiên bản go1.18.2 linux/amd64
```

### Cài đặt node Celestia

Cài đặt binary nodes celestia bằng cách chạy các lệnh sau:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Xác minh rằng binary đang hoạt động và kiểm tra phiên bản với lệnh phiên bản celestia:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Khởi tạo light node

Chạy lệnh sau:

```sh
celestia light init
```

Bạn sẽ thấy kết quả sau:

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### Khởi động light node

Khởi động Bridge Node bằng kết nối với điểm cuối gRPC của node valiator (thường được hiển thị trên cổng 9090):

> LƯU Ý: Để có quyền truy cập vào khả năng nhận/ gửi thông tin liên quan đến trạng thái, chẳng hạn như gửi các giao dịch PayForData hoặc truy vấn số dư tài khoản của node, điểm cuối gRPC của node validator (cốt lõi) phải được chuyển như hướng dẫn bên dưới.

```sh
celestia light start --core.grpc http://<ip>:9090
```

Nếu bạn cần danh sách các điểm cuối RPC để kết nối, bạn có thể kiểm tra từ danh sách [ here ](./mamaki-testnet.md#rpc-endpoints)

Ví dụ: lệnh của bạn có thể trông giống như sau:

```sh
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090
```

### Mã khóa và ví

Bạn có thể tạo mã khóa cho node của mình bằng cách chạy lệnh sau:

```sh
make cel-key
```

Khi bạn khởi động Light Node, một mã khóa ví sẽ được tạo. Bạn sẽ cần nạp tiền cho địa chỉ đó bằng mã thông báo Mamaki Testnet để thanh toán cho Giao dịch PayForData.

Bạn có thể tìm thấy địa chỉ bằng cách chạy lệnh sau trong thư mục ` celestia-node `:

```sh
./cel-key list --node.type light --keyring-backend test
```

Yêu cầu mã thông báo Mamaki Testnet tại [here](./mamaki-testnet.md#mamaki-testnet-faucet).

### Tùy chọn: chạy light node bằng khóa tùy chỉnh

Để chạy một light node bằng khóa tùy chỉnh:

1. Khóa tùy chỉnh phải tồn tại bên trong thư mục celestia bridge node tại đúng đường dẫn (default: ` ~ /.celestia-light / keys / keyring-test `)
2. Tên của khóa tùy chỉnh phải được chuyển khi ` start `, như sau:

```sh
celestia light start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Tùy chỉnh: bắt đầu light node với SystemD

Làm theo hướng dẫn về cách thiết lập light node làm quy trình nền với SystemD [ here](./systemd.md#celestia-light-node).

## Mẫu tính khả dụng của dữ liệu (DAS)

Khi light node đang chạy, bạn có thể xem hướng dẫn này trong việc gửi ` PayForData ` transaction [ here ](../developers/node-tutorial.md).
