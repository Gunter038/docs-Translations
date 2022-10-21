---
sidebar_label: 合约部署
---

# 使用 Rollmint 在 CosmWasm 上部署合约
<!-- markdownlint-disable MD013 -->

## 编译智能合约

我们将运行以下命令来拉取命名服务智能合约并进行编译：

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

编译后的合约输出到： `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`。

## 单元测试

如果我们想运行测试，我们可以使用以下命令：

```sh
cargo unit-test
```

## Optimized 智能合约

因为我们正在将编译后的智能合约部署到 `wasmd `，所以我们希望它的空间尽可能的小。

CosmWasm 团队为我们提供了一个叫做`rust-optimizer`的工具 ，我们需要 Docker 才能编译它。

运行以下命令：

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

这会将优化好的 Wasm 字节码放置在 `artifacts/cw_nameservice.wasm`。

## 合约部署

现在让我们部署我们的智能合约！

运行以下命令：

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

这将为您提供智能合约部署的交易哈希。 鉴于我们使用的是 Rollmint，所以 Rollmint 在 Celestia 的数据可用性层上等待确认该块已被包含，然后再提交新块，因此包含交易将会有延迟。
