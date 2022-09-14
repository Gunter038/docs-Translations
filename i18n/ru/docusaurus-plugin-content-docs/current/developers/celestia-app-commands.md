- - -
sidebar_label : Полезные команды CLI
- - -

# Полезные команды CLI

Параметры отображения:

```console
$ celestia-appd --help
Запуск приложения celestia
Использование:
  celestia-appd [команда]

Available Commands:
  add-genesis-account Добавить исходную учетную запись в genesis.json
  collect-gentxs      Собрать исходные транзакции и вывести файл genesis.json
  config              Создать или запросить файл конфигурации приложения CLI
  debug               Инструмент для помощи в отладке вашего приложения
  export              Экспорт состояния в JSON
  gentx               Создать исходную транзакцию с самостоятельной делегацией
  help                Помощь с любой командой
  init                Инициализация файлов приватного валидатора, p2p, исходного, и конфигурации приложения
  keys                Управление ключами вашего приложения
  migrate             Миграция исходного файла на определенную версию
  query               Запрос подкоманд
  rollback            откат состояния тендерминта на одну высоту
  rollback            откат состояния cosmos-sdk и тендерминта на одну высоту
  start               Запуск полного узла
  status              Запрос статуса от удаленного узла
  tendermint          Подкоманы тендерминта
  tx                  Подкоманды транзакций
  validate-genesis    проверяет исходный файл в месте по умолчанию 
  или в месте, переданном в качестве аргумента
  version             Вывод информации о бинарной версии приложения
```

## Создание кошелька

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` настраивает серверную часть связки ключей, где хранятся ключи.

Параметры: `os|file|kwallet|pass|test|memory`.

## Управление ключами

```sh
# список ключей
celestia-appd keys list

# добавление ключей
celestia-appd keys add <KEY_NAME>

# удаление ключей
celestia-appd keys delete <KEY_NAME>

# переименование ключей
celestia-appd keys rename <CURRENT_KEY_NAME> <NEW_KEY_NAME>
```

### Импорт и экспорт ключей

Импорт зашифрованного закрытого ключа в формате ASCII в локальную базу ключей.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Example usage:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Export a private key from the local keyring in encrypted and ASCII-armored format:

```sh
celestia-appd keys export <KEY_NAME>

# you will then be prompted to set a password for the encrypted private key:
Enter passphrase to encrypt the exported key:
```

After you set a password, your encrypted key will be displayed.

## Querying subcommands

Usage:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

To see all options:

```sh
celestia-appd q --help
```

## Token management

Get token balances:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Example usage:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Transfer tokens from one wallet to another:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Example usage:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

To see options:

```sh
celestia-appd tx bank send --help
```
