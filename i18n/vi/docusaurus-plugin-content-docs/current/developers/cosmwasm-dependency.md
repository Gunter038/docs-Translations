---
sidebar_label: CosmWasm Dependencies
---

# Cài đặt các thành phần phụ thuộc của CosmWasm

## Thiết lập môi trường

Đối với hướng dẫn này, chúng ta sẽ sử dụng `curl` và `jq` như là một công cụ hữu ích.

Bạn có thể làm theo hướng dẫn sau để cài đặt chúng [tại đây](./environment.md#setting-up-dependencies).

## Thành phần phụ thuộc Golang

Phiên bản Golang được sử dụng cho hướng dẫn này là v1.18+

Nếu bạn đang sử dụng Linux, bạn có thể cài Golang bằng cách làm theo hướng dẫn sau đây của chúng tôi [tại đây](./environment.md#install-golang).

## Cài đặt Rust

### Rustup

Đầu tiên, trước khi cài đặt Rust, bạn sẽ cần phải cài đặt `rustup`.

Đối với Mac/Linux, sau đây là câu lệnh để cài đặt nó:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Sau khi cài đặt, chạy những câu lệnh sau đây để thiết lập Rust.

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

Ở đây, chúng tôi sẽ sử dụng kho lưu trữ `wasmd` và thay thế Tendermint bằng Rollmint. Rollmint là một sự thay thế drop-in cho Tendermint cho phép ứng dụng Cosmos-SDK để kết nối với mạng Data Availability của Celestia.

```sh
git clone https://github.com/CosmWasm/wasmd.git
cd wasmd
git fetch --tags
git checkout v0.27.0
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy 
go mod download
make install
```
