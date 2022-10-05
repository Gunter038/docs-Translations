- - -
sidebar_label : Thiết lập môi trường
- - -

# Môi trường phát triển

Hướng dẫn này sẽ xem xét thiết lập môi trường phát triển của bạn để chạy Phần mềm Celestia. Môi trường này có thể được sử dụng để phát triển, xây dựng binary và chạy nodes.

## Cài đặt các dependency

Một khi bạn đã thiết lập máy của mình, ssh vào máy của bạn để bắt đầu cài đặt dependencies cần thiết để chạy một node.

Đầu tiên, hãy bảo đảm đã cập nhật và nâng cấp OS:

```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt update && sudo apt upgrade -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum update
```

Chúng là những gói thiết yếu, cần thiết để thực thi nhiều tác vụ ví dụ như tải file, biên dịch, và quản lý node:

<!-- markdownlint-disable MD013 -->
```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Cài đặt Golang

Celestia-app và celestia-node được viết bằng [ Golang ](https://go.dev/) do vậy chúng tôi phải cài đặt Golang để thiết lập và chạy nó.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Hiện tại chúng tôi cần thêm `/usr/local/go/bin` directory to `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Kiểm tra xem Go đã được cài đặt để chạy đúng chưa:

```sh
phiên bản
```

Đầu ra phải là phiên bản được cài đặt:

```sh
go version go1.19.1 linux/amd64
```
