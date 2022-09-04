- - -
sidebar_label : Create A Celestia Testnet
- - -

# Celestia App Network Instantiation Guide

This guide is for helping instantiate a new testnetwork and following the correct steps to do so with Celestia-App. You should only follow this guide if you want to experiment with your own Celestia Testnetwork or if you want to test out new features to build as a core developer.

## Вимоги до обладнання

You can follow hardware requirements [here](../nodes/validator-node.md#hardware-requirements).

## Налаштування залежностей

Ви можете налаштувати залежності, дотримуючись інструкції [тут](./environment.md).

## Celestia App Installation

Ви можете встановити додаток Celestia за інструкцією [тут](./celestia-app.md).

## Spin Up A Celestia Testnet

If you want to spin up a quick testnet with your friends, you can follow these steps. Unless otherwise noted, every step must be done by everyone who wants to participate in this testnet.

### Optional: Reset Working Directory

If you have already initialized a working directory for `celestia-appd` in the past, you must clean up before reinitializing a new directory. Ви можете зробити це, виконавши таку команду:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Ініціалізувати Робочий каталог

Запустіть таку команду:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* Значення, яке ми будемо використовувати для `$VALIDATOR_NAME`, це `validator1`, але вам слід вибрати власну назву ноди.
* Значення, яке ми використовуватимемо для `$CHAIN_ID` є `testnet`. `$CHAIN_ID` має залишатися незмінним для всіх учасників цієї мережі.

### Створити новий ключ

Далі запустіть таку команду:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

Це створить новий ключ із назвою, яку ви оберете. Збережіть десь вивід цієї команди; згенерована тут адреса знадобиться вам пізніше. Тут ми задаємо значення нашого ключа `$KEY_NAME` у `validator` для демонстрації.

### Додати ім'я облікового запису Genesis

Запустіть таку команду:

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Тут `$VALIDATOR_NAME` має таке ж ключове ім'я, як і раніше; і `$AMOUNT` це щось на зразок `1000000000000000000000utia`.

### Необов'язково: Додавання інших валідаторів

Якщо інші учасники вашого тесту також хочуть бути валідаторами, повторіть наведену вище команду із зазначенням конкретної суми для їх відкритих ключів.

Після додавання всіх валідаторів буде створено файл `genesis.json`. Вам потрібно поділитися ним з усіма іншими валідаторами у вашій тестовій мережі, щоб усі могли виконати наступний крок.

Ви можете знайти `genesis.json` в `$HOME/.celestia-app/config/genes.json`

### Створіть транзакцію Genesis для нового ланцюга

Запустіть таку команду:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

Це створить транзакцію Genesis для вашого нового ланцюга. Тут `$STAKING_AMOUNT` має бути як мінімум `1000000000utia`. Якщо ви надасте занадто багато або занадто мало, ви зіткнетеся з помилкою під час запуску вашої ноди.

Ви знайдете згенерований файл gentx JSON всередині `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Примітка: Якщо у вашій мережі є інші валідатори, їм також потрібно виконати наведену вище команду з файлом `genesis.json`, доступ до якого ви надали їм у попередньому кроці.

### Створення файлу Genesis JSON

Після того, як усі учасники надішлють вам свої файли gentx JSON, ви затягнете всі ці файли gentx у такий каталог: `$HOME/.celestia-appd/config/gentx` і використаєте їх для створення остаточного файлу `genesis.json`.

Додавши файли gentx усіх учасників, виконайте таку команду:

```sh
celestia-appd collect-gentxs
```

Ця команда шукатиме у цьому сховищі файли gentx, які слід перемістити до наступного каталогу `$HOME/.celestia-app/config/gentx`.

Це оновить файл `genesis.json` у цьому місці `$HOME/.celestia-app/config/genesis.json`, яке тепер містить gentx інших учасників.

Потім вам слід поділитися цим остаточним файлом `genesis.json` з усіма іншими учасниками, які мають додати його до свого каталогу `$HOME/.celestia-app/config`.

Кожен повинен переконатися, що вони замінили свій наявний файл `genesis.json` новим створеним.

### Змінити ваш файл конфігурації

Відкрийте файл `$HOME/.celestia-app/config/config.toml`, щоб модифікувати його.

Всередині файлу додайте інших учасників, змінивши наступний рядок, щоб увімкнути інших учасників як постійних:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

You can find `validator_address` by running the following command:

```sh
celestia-appd tendermint show-node-id
```

The output will be the hex-encoded `validator_address`. The default `port` is 26656.

### Instantiate the Network

You can start your node by running the following command:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
