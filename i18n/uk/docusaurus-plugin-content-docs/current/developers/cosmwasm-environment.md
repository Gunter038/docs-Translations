---
sidebar_label: Налаштування середовища мережі
---

# Налаштування вашого середовища для CosmWasm на Celestia
<!-- markdownlint-disable MD013 -->

Тепер двійковий файл `wasmd` створено, нам потрібно налаштувати локальну мережу, яка обмінюється даними між `wasmd` і Optimint.

## Побудова мережі Wasmd

Запустіть таку команду:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Це ініціалізує ланцюжок `celeswasm` з двійковим `wasmd`.

Наступна команда допомагає нам налаштувати облікові записи для genesis:

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Переконайтеся, що ви зберігаєте вихідні дані гаманця, згенеровані для подальшого використання, якщо це необхідно.

Тепер додаймо обліковий запис genesis і використаймо його для оновлення нашого файлу genesis:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Завдяки цьому ми створили файл genesis локальної мережі.

Ще кілька корисних команд, які ми можемо налаштувати:

```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```

## Запуск мережі Wasmd

Ми можемо виконати наступне, щоб запустити мережу `wasmd`:

```sh
wasmd start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Будь ласка, зверніть увагу:

> ПРИМІТКА. У наведеній вище команді вам потрібно передати IP-адресу вузла Celestia до `base_url`, який має обліковий запис із маркерами тестової мережі Mamaki. Дотримуйтеся посібника з налаштування Celestia Light Node і створення гаманця з тестовими токенами [тут](./node-tutorial.md) у розділі Celestia Node.

Також зверніть увагу:

> ВАЖЛИВО: Крім того, у наведеній вище команді вам потрібно вказати останню висоту блоку в тестовій мережі Mamaki для `da_height`. Ви можете знайти останній номер блоку в провіднику [тут](https://testnet.mintscan.io/celestia-testnet). Крім того, для прапора `--optimint.namespace_id` ви можете згенерувати випадковий ідентифікатор простору імен за допомогою ігрового майданчика [тут](https://go.dev/play/p/7ltvaj8lhRl)

Таким чином ми запустили нашу мережу `wasmd`!
