---
sidebar_label: Развертывание контракта
---

# Contract Deployment on CosmWasm with Rollmint
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

Команда CosmWasm предоставляет инструмент под названием `rust-optimizer`, для запуска которого нам необходим Docker.

Выполните следующую команду:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Это позволит поместить оптимизированный байт-код Wasm в `artifacts/cw_nameservice.wasm`.

## Развертывание контракта

Давайте теперь развернем наш смарт контракт!

Выполните следующее:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Это позволит вам получить хэш транзакции для развертывания смарт контракта. Given we are using Rollmint, there will be a delay on the transaction being included due to Rollmint waiting on Celestia's Data Availability Layer to confirm the block has been included before submitting a new block.
