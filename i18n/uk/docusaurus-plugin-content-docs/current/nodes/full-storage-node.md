- - -
sidebar_label : Нода повного сховища
- - -

# Налаштування ноди повного сховища Celestia

Цей туторіал допоможе вам налаштувати ноду повного сховища Celestia, тобто ноду Celestia, яка не підключається до Celestia App (тому не є повною нодою), але зберігає всі дані.

## Вимоги до обладнання

Для роботи ноди повного сховища є мінімальні вимоги до обладнання:

* Оперативна пам’ять: 8 GB
* CPU: Quad-Core
* Пам’ять на диску: 250 GB
* Пропускна здатність: 1 Gbps для загрузки/100 Mbps для вивантаження

## Налаштування ноди повного сховища

Цей туторіал виконано на машині Ubuntu Linux 20.04 (LTS) x64.

### Налаштування залежностей

Ви можете використати [цей туторіал](../developers/environment.md) по налаштуванню залежностей

## Встановлення ноди Celestia

> Примітка. Переконайтеся, що у вас є принаймні 250+ Гб вільного місця для ноди повного сховища Celestia

Ви можете використати [цей туторіал](../developers/celestia-node.md) із встановлення ноди Celestia

### Запуск ноди повного сховища

#### Ініціалізація ноди повного сховища

Виконайте таку команду:

```sh
celestia full init
```

#### Запуск ноди повного сховища

Запустіть ноду повного сховища з підключенням до кінцевої точки gRPC ноди валідатора (яка зазвичай доступна на порту 9090):

> ПРИМІТКА. Щоб отримати доступ до можливості отримати/надсилати інформацію, пов’язану зі станом, наприклад можливість надсилати транзакції PayForData або запитувати баланс рахунку ноди, кінцеву точку gRPC ноди валідатора (основного) потрібно передати відповідно до вказівок нижче.

A note on ports:

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port>
```
<!-- markdownlint-enable MD013 -->

Якщо ви хочете знайти приклад кінцевих точок RPC, перегляньте список ресурсів [тут](./mamaki-testnet.md#rpc-endpoints).

Ви можете створити ключ для своєї ноди, дотримуючись інструкцій `cel-key` [тут](./keys.md)

Після запуску повної ноди для вас буде згенеровано ключ гаманця. You will need to fund that address with testnet tokens to pay for PayForData transactions. Ви можете знайти адресу, виконавши таку команду:

```sh
./cel-key list --node.type full --keyring-backend test
```

You have two networks to get testnet tokens from:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a full-storage node for your sovereign rollup, it is highly recommended to request Arabica devnet tokens as Arabica has the latest changes that can be used to test for developing your sovereign rollup. You can still use Mamaki Testnet as well, it is just mostly used for Validator operations.

### Додатково: запуск ноди повного сховища за допомогою кастомного ключа

Щоб запустити повну ноду зберігання за допомогою кастомного ключа:

1. Кастомний ключ має існувати в каталозі повної ноди зберігання celestia за правильним шляхом (за замовчуванням: `~/.celestia-full/keys/keyring-test`)
2. Ім'я кастомного ключа має бути передано під час `запуску`, наприклад:

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port> --keyring.accname <name-of-custom-key>
```
<!-- markdownlint-enable MD013 -->

### Додатково: запуск ноди повного сховища за допомогою SystemD

Дотримуйтеся [цього туторіала](./systemd.md#celestia-full-storage-node) з налаштування повної ноди зберігання як фонового процесу за допомогою SystemD.

Таким чином, ви тепер використовуєте ноду повного сховища Celestia.
