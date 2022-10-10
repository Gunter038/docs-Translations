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

Пример использования:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Экспорт закрытого ключа из локальной связки ключей в зашифрованном и защищенном формате ASCII:

```sh
celestia-appd keys export <KEY_NAME>:
# вам будет предложено установить пароль для зашифрованного закрытого ключа:
Введите парольную фразу для шифрования экспортированного ключа:
```

После установки пароля будет показан зашифрованный ключ.

## Запрос подкоманд

Использование:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

Чтобы увидеть все параметры:

```sh
celestia-appd q --help
```

## Управление токенами

Баланс токенов:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Пример использования:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Перевод токенов с одного кошелька на другой:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Пример использования:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

Чтобы увидеть все параметры:

```sh
celestia-appd tx bank send --help
```

## Governance

You can vote on a governance proposal with the following command:

```sh
celestia-appd tx gov vote <proposal id> <yes or no> --from <wallet> --chain-id <chain-id>
```

## Claim validator rewards

You can claim your validator rewards with the following command:

```sh
celestia-appd tx distribution withdraw-rewards <validator valoper>\
    --commission --from=<validator wallet> --chain-id <chain-id> --gas auto -y
```

## Delegate & undelegate tokens

You can `delegate` your tokens to a validator with the following command:

```sh
celestia-appd tx staking delegate <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

You can undelegate tokens to a validator with the `unbond` command:

```sh
celestia-appd tx staking unbond <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

## Unjailing the validator

You can unjail your validator with the following command:

```sh
celestia-appd tx slashing unjail --from <validator wallet>\
    --chain-id <chain-id> --gas auto -y
```

## How to export logs with SystemD

You can export your logs if you are running a SystemD service with the following command:

```sh
sudo journalctl -u <your systemd service> -S yesterday > node_logs.txt
sudo journalctl -u <your systemd service> -S today > node_logs.txt
# This command outputs the last 1 million lines!
sudo journalctl -u <your systemd service> -n 1000000 > node_logs.txt
```
