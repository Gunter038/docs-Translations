---
sidebar_label: CosmWasm 依赖项
---

# CosmWasm 依赖项安装

## 环境配置

对于本教程，我们将使用 `curl` 和 `jq` 作为有用的工具。

您可以按照[这里](./environment.md#setting-up-dependencies)的安装指南进行操作。

## Golang 依赖

本教程使用的 Golang 版本是 v1.18+

如果您使用的是 Linux 发行版，则可以按照[这里](./environment.md#install-golang)的教程安装 Golang 。

## Rust 安装

### Rustup

首先，在安装Rust之前，您需要安装 `rustup`。

在 Mac/Linux 系统上，以下是安装它的命令：

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装完成后，按此命令来设置 Rust。

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Docker 安装

我们将在本教程后面使用 Docker 来编译智能合约以使用较小的内存。

我们建议在您的机器上安装 Docker。

有关如何在 Linux 上安装它的示例，请参见[此处](https://docs.docker.com/engine/install/ubuntu/)。

找到您操作系统的正确说明。

## wasmd 安装

Here, we are going to pull down the `wasmd` repository and replace Tendermint with Rollmint. Rollmint is a drop-in replacement for Tendermint that allows Cosmos-SDK applications to connect to Celestia's Data Availability network.

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
