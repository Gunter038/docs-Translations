- - -
sidebar_label : Setting Up Environment
- - -

# Development Environment

This tutorial will go over setting up your development environment to run Celestia software. This environment can be used for development, building binaries, and running nodes.

## Install Dependencies

Once you have setup your instance, ssh into the instance to begin installing the dependencies needed to run a node.

First, make sure to update and upgrade the OS:

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

These are essential packages that are necessary to execute many tasks like downloading files, compiling, and monitoring the node:

<!-- markdownlint-disable MD013 -->
```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Install Golang

Celestia-app và celestia-node được viết bằng [ Golang ](https://go.dev/) do vậy chúng tôi phải cài đặt Golang để thiết lập và chạy nó.

```sh
ver="1.18.2"
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
vào phiên bản go1.18.2 linux/amd64
```
