---
sidebar_label: Настройка сетевого окружения
---

# Настройка среды для CosmWasm на Celestia

Теперь, когда бинарный файл `wasmd` собран, нам нужно настроить локальную сеть, которая будет взаимодействовать между `wasmd` и Rollmint.

## Создание сети Wasmd

Выполните следующие команды:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Это инициализирует сеть под названием `celeswasm` с бинарным файлом `wasmd`.

Следующая команда поможет нам настроить генезис аккаунт:

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Обязательно сохраните вывод мнемонической фразы, сгенерированного кошелька для последующего использования в случае необходимости.

Теперь давайте добавим аккаунт генезиса и используем его для обновления нашего файла генезиса:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

С помощью этого мы создали файл генезиса в локальной сети.

Еще несколько полезных команд, которые мы можем использовать для настройки:

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## Запуск сети Wasmd

Мы можем выполнить следующее для запуска сети `wasmd`:

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Пожалуйста, обратите внимание:

> ПРИМЕЧАНИЕ: В приведенной выше команде в `base_url` необходимо передать IP-адрес ноды Celestia, которая имеет аккаунт с токенами Arabica в Devnet. Руководство по настройке лайт ноды Celestia и по созданию кошелька с тестовыми токенами вы можете найти[здесь](./node-tutorial.md) в разделе "Нода Celestia".

Также просим учесть:

> ВАЖНО: Кроме того, в приведенной выше команде необходимо указать последнюю высоту блока в Arabica в Devnet для `da_height`. Вы можете найти номер последнего блока в эксплорере [здесь](https://explorer.celestia.observer/arabica). Также, для флага `--rollmint.namespace_id`, вы можете сгенерировать случайный Namespace ID, используя программу [здесь](https://go.dev/play/p/7ltvaj8lhRl)

Таким образом, мы запустили нашу `wasmd` сеть!
