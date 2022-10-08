- - -
sidebar_label : Корисні CLI команди
- - -

# Корисні CLI команди

Переглянути всі варіанти:

```console
$ celestia-appd --help
Start celestia app

Використання:
celestia-appd [команда]

Доступні команди:
  add-genesis-account Додайте обліковий запис genesis до genesis.json
  collect-gentxs      Зберіть файл genesis txs і виведіть файл genesis.json
  config              Створіть або запитайте файл конфігурації CLI програми
  debug               Інструмент для допомоги у налагодженні програми
  export              Експортувати стан до JSONa
  gentx               Створити генезис tx із самоделегуванням
  help                Довідка про будь-яку команду
  init                Ініціалізувати приватний валідатор, p2p, genesis і файли конфігурації програми
  keys                Керування ключами програми
  migrate             Перенести Genesis до вказаної цільової версії
  query               Запит
  rollback            Відкотити стан tendermint на одну висоту
  rollback            Відкотити cosmos-sdk і стан tendermint на одну висоту
  start               Запуск повної ноди
  status              Запитати стан віддаленого вузла
  tendermint          Підкоманди Tendermint
  tx                  Підкоманди транзакцій
  validate-genesis    перевіряє файл генезису в розташуванні за замовчуванням або в розташуванні, переданому як аргумент
  version             Роздрукуйте інформацію про двійкову версію програми
```

## Створення гаманця

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` налаштовує серверну частину брелоків, де зберігаються ключі.

Варіанти є: `os|file|kwallet|pass|test|memory`.

## Управління ключем

```sh
# listing keys
celestia-appd keys list

# adding keys
celestia-appd keys add <KEY_NAME>

# deleting keys
celestia-appd keys delete <KEY_NAME>

# renaming keys
celestia-appd keys rename <CURRENT_KEY_NAME> <NEW_KEY_NAME>
```

### Імпорт і експорт ключів

Імпортуйте зашифрований приватний ключ із кодуванням ASCII у локальну базу ключів.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Приклад використання:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Експортуйте приватний ключ із локального зв’язку ключів у зашифрованому та ASCII-броньованому форматі:

```sh
celestia-appd keys export <KEY_NAME>

# потім вам буде запропоновано встановити пароль для зашифрованого закритого ключа:
Введіть парольну фразу для шифрування експортованого ключа:
```

Після встановлення пароля відобразиться ваш зашифрований ключ.

## Підкоманди запиту

Використання:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

Щоб побачити всі варіанти:

```sh
celestia-appd q --help
```

## Управління токеном

Отримати баланси токенів:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Приклад використання:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Відправити токени з одного гаманця на інший:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Приклад використання:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

Щоб переглянути параметри:

```sh
celestia-appd tx bank send --help
```

## Управління

Ви можете голосувати за пропозицію управління за допомогою такої команди:

```sh
celestia-appd tx gov vote <proposal id> <yes or no> --from <wallet> --chain-id <chain-id>
```

## Запит винагороди валідатора

Ви можете отримати винагороди валідатора за допомогою такої команди:

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
