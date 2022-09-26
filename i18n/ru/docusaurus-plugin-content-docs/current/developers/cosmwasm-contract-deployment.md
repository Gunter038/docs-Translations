---
sidebar_label: Contract Deployment
---

# Развертывание контрактов на CosmWasm с помощью Optimint
<!-- markdownlint-disable MD013 -->

## Компиляция смарт-контракта

Мы выполним следующие команды, чтобы вызвать смарт-контракт Nameservice и скомпилировать его:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Вывод скомпилированного контракта: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Модульные тесты

Если мы хотим провести тесты, мы можем сделать это с помощью следующей команды:

```sh
cargo unit-test
```

## Оптимизированный смарт контракт

Поскольку мы развертываем скомпилированный смарт контракт на `wasmd`, то хотелось б, чтобы он был как можно меньше.

CosmWasm team provides a tool called `rust-optimizer` which we need Docker for in order to compile.

Run the following command:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

This will place the optimized Wasm bytecode at `artifacts/cw_nameservice.wasm`.

## Contract Deployment

Let's now deploy our smart contract!

Run the following:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

This will get you the transaction hash for the smart contract deployment. Given we are using Optimint, there will be a delay on the transaction being included due to Optimint waiting on Celestia's Data Availability Layer to confirm the block has been included before submitting a new block.
