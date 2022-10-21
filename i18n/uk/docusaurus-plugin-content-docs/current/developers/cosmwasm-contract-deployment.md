---
sidebar_label: Контрактне розгортання
---

# Контрактне розгортання на CosmWasm з Rollmint
<!-- markdownlint-disable MD013 -->

## Компільовані розумні контракти

Ми запустимо наступні команди, щоб отримати смарт-контракт Nameservice і скомпілювати його:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Зкомпільований контракт виводиться на: `target/wasm32-unknown-unknown/release/cw_nameservice.m`.

## Модульні тести

Якщо ми хочемо запустити тести, то ми можемо зробити це за допомогою наступної команди:

```sh
cargo unit-test
```

## Оптимізований смарт-контракт

Оскільки ми розгортаємо скомпільований смартконтракт у `wasmd`, ми хочемо, щоб він був якомога меншим.

Команда CosmWasm пропонує інструмент під назвою `rust-optimizer`, для компіляції якого нам потрібен Docker.

Виконайте таку команду:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Це розмістить оптимізований байт-код Wasm у `artifacts/cw_nameservice.wasm`.

## Контрактне розгортання

Давайте тепер розгорнемо наш смартконтракт!

Пропишіть наступне:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Це дозволить вам отримати хеш транзакції для розгортання смартконтракту. Оскільки ми використовуємо Rollmint, буде затримка включення транзакції через те, що Rollmint чекає на рівні доступності даних Celestia, щоб підтвердити, що блок було включено, перш ніж надсилати новий блок.
