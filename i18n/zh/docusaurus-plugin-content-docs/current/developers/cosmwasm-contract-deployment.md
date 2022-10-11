---
sidebar_label: 合约部署
---

# CosmWasm与Rollmint的合约部署
<!-- markdownlint-disable MD013 -->

## 编译智能合约

我们将运行以下命令来拉取 Nameservice 智能合约并进行编译：

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

编译后的合约输出到：`target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## 部署测试

如果我们想运行测试，我们可以使用以下命令：

```sh
cargo unit-test
```

## 优化智能合约

因为我们正在将编译后的智能合约部署到 `wasmd `，所以我们希望它的空间尽可能的小。

CosmWasm 团队提供了一个工具 `rust-optimizer` ，我们需要 Docker 来编译它。

运行以下命令：

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

这会将优化的 Wasm 字节码放置在`artifacts/cw_nameservice.wasm`.

## 部署合约

现在让我们部署我们的智能合约！

运行以下命令：

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

这将为您提供智能合约部署的交易哈希。 鉴于 我们正在使用Rollmint，包括交易会有延迟 由于Rollmint正在等待Celestia的数据可用性层确认数据块 在提交新块之前已包含。
