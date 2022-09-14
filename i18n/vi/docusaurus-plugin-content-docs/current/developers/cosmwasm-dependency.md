---
sidebar_label: CosmWasm Dependencies
---

# CosmWasm Dependency Installations

## Environment Setup

For this tutorial, we will be using `curl` and `jq` as helpful tools.

You can follow the guide on installing them [here](./environment.md#setting-up-dependencies).

## Golang Dependency

The Golang version used for this tutorial is v1.18+

If you are using a Linux distribution, you can install Golang by following our tutorial [here](./environment.md#install-golang).

## Rust Installation

### Rustup

First, before installing Rust, you would need to install `rustup`.

On Mac/Linux systems, here are the commands for installing it:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

After installation, follow the commands here to setup Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Cài đặt Docker

Chúng tôi sẽ sử dụng Docker sau trong hướng dẫn này soạn hợp đồng thông minh để sử dụng một small footfrint.

Chúng tôi khuyên cài đặt Docker trên máy tính của mình.

Ví dụ về cách cài đặt nó trên Linux có tại [here ](https://docs.docker.com/engine/install/ubuntu/).

Tìm các hướng dẫn phù hợp cụ thể cho hệ điều hành của bạn.

## wasmd Installation

Ở đây, chúng tôi sẽ kéo xuống kho lưu trữ ` wasmd ` và thay thế Tendermint bằng Optimint. Optimint là một drop-in replacemen cho Tendermint cho phép ứng dụng Cosmos-SDK kết nối với mạng tính khả dụng dữ liệu của Celestia.

```sh
git clone https://github.com/CosmWasm/wasmd.git
cd wasmd
git fetch --tags
git checkout v0.27.0
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy 
go mod download
make install
```
