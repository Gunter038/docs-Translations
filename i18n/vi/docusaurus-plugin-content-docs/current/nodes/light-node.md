- - -
sidebar_label : Light Node
- - -

# Thiết lập Light Node Celestia

Hướng dẫn này sẽ chỉ bạn cách thiết lập một Light Node Celestia. Việc này sẽ cho phép bạn thực hiện việc lấy mẫu Data Availability (DA) trên mạng Data Availability.

> Để xem video hướng dẫn cách thiết lập light node Celestia, bấm [vào đây](../developers/light-node-video.md)

## Tổng quan về light node

Light Node giúp bảo đảm Data availability. Đây là cách phổ biến nhất để tương tác với mạng Celestia.

![light-node](/img/nodes/LightNodes.png)

Light nodes có thực hiện những hành động sau:

1. Chúng lắng nghe ExtendedHeaders, ví dụ wrapped “raw” headers, thứ thông báo cho các Celestia node về các block header mới và metadata DA liên quan.
2. Chúng thực hiện Lấy mẫu Data Availability (DAS) trên các header nhận được

## Yêu cầu phần cứng

Yêu cầu phần cứng tối thiểu bên dưới đây được khuyến nghị để chạy một light node:

* Memory: 2 GB RAM
* CPU: Single Core
* Disk: 5 GB SSD Storage
* Bandwidth: 56 Kbps đối với Download/56 Kbps đối với Upload

## Thiết lập light node của bạn

Hướng dẫn này sẽ được thực hiện trên thiết bị Ubuntu Linux 20.04 (LTS) x64.

### Thiết lập các thành phần phụ thuộc

Đầu tiên, hãy bảo đảm đã cập nhật và nâng cấp OS:

```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt update && sudo apt upgrade -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum update
```

Chúng là những gói thiết yếu, cần thiết để thực thi nhiều tác vụ ví dụ như tải file, phiên dịch, và quản lý node:

```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### Cài đặt Golang

Celestia-app và celestia-node được viết bằng ngôn ngữ lập trình [Golang](https://go.dev/) do vậy chúng ta phải cài đặt Golang để thiết lập và chạy nó.

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Bây giờ chúng ta cần thêm thư mục `/usr/local/go/bin` vào `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Để kiểm tra xem Go đã được cài đặt để chạy đúng chưa:

```sh
go version
```

Đầu ra phải là phiên bản được cài đặt:

```sh
go version go1.18.2 linux/amd64
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
