---
sidebar_label: CosmWasm依赖
---

# CosmWasm Dependency Installations

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

We will be using Docker later in this tutorial for compiling a smart contract to use a small footprint.

We recommend installing Docker on your machine.

Examples on how to install it on Linux are found [here](https://docs.docker.com/engine/install/ubuntu/).

Find the right instructions specific for your OS.

## wasmd Installation

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
