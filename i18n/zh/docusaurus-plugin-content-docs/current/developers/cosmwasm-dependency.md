---
sidebar_label: CosmWasm依赖
---

# CosmWasm依赖安装

## 环境配置

在本教程中，我们会使用 `curl` 和 `jq` 作为有效工具。

你可以按照这个指南进行安装 [这里](./environment.md#setting-up-dependencies).。

## Golang依赖

本教程使用的Golong版本是 v1.18+

如果你用的是Linux发行版，你可以通过关注我们教程来安装GoLang [这里](./environment.md#install-golang)。

## Rust安装

### Rustup

首先，在安装Rust之前，你需要安装 `rustup`。

对于Mac/Linux系统，这里是它们的安装命令：

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

安装之后，按这个命令来设置Rust。

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Docker安装

我们会在教程后面用Docker以较少内存来编制智能合约。

我们建议在你的机器上安装Docker。

如何在Linxu上安装的例子，参见[这里](https://docs.docker.com/engine/install/ubuntu/)。

为你的操作系统找到正确的指南。

## wasmd安装

在这里，我们会删除`wasmd` 存储库，并用Rollmint替换Tendermint。 Rollmint是Tendermin随时可用的替代品，允许Cosmos-SDK应用连接Celestia的数据可用性网络层。

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
