---
sidebar_label: Контрактне розгортання
---

# Контрактне розгортання на CosmWasm з Optimint
<!-- markdownlint-disable MD013 -->

## Компілюйте смартконтракт

Ми запустимо наступні команди, щоб отримати смарт-контракт Nameservice і скомпілювати його:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Зібраний контракт виводиться до: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Модульні тести

Якщо ми хочемо запустити тести, то ми можемо зробити це за допомогою наступної команди:

```sh
cargo unit-test
```

## Оптимізований смартконтракт

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

## Розгортання контракту

Давайте тепер розгорнемо наш смартконтракт!

Пропишіть наступне:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Це дозволить вам отримати хеш транзакції для розгортання смартконтракту. Оскільки ми використовуємо Optimint, буде затримка включення транзакції через те, що Optimint чекає на рівні доступності даних Celestia, щоб підтвердити, що блок було включено, перш ніж надсилати новий блок.
