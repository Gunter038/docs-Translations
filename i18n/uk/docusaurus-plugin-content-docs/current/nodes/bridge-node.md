- - -
sidebar_label : Bridge Node
- - -

# Налаштування Мостової Ноди Celestia

У цьому туторіалі описано кроки з налаштування вашої ноди для мосту Celestia.

Мостові ноди з’єднують рівень доступності даних і рівень консенсусу, а також мають можливість стати валідатором.

Валідатори не повинні запускати мостові ноди, але це рекомендується, щоб передавати блоки в мережу доступності даних.

## Огляд мостових нод

Мостова нода Celestia має такі властивості:

1. Імпортуйте та обробляйте «необроблені» заголовки та блоки з надійного основного процесу (тобто надійного RPC-з’єднання з нодою celestia-core) у мережі консенсусу. Мостові ноди можуть запускати цей основний процес внутрішньо (вбудовано) або просто підключатися до віддаленої кінцевої точки. Мостові ноди також мають можливість бути активним валідатором у мережі консенсусу.
2. Перевіряйте та видаляйте код «необроблених» блоків
3. Надавайте спільні блоки із заголовками доступності даних для легких нод у мережі DA. ![bridge-node-diagram](/img/nodes/BridgeNodes.png)

З точки зору реалізації, мостові ноди запускають два окремих процеси:

1. Додаток Celestia з Celestia Core ([репозиторій](https://github.com/celestiaorg/celestia-app))

    * **Додаток Celestia** — це кінцевий механізм, у якому запускається програма та логіка proof-of-stake. Додаток Celestia створено на основі [Cosmos SDK](https://docs.cosmos.network/), а також містить **Celestia Core**.
    * **Celestia Core** — це рівень взаємодії стану, консенсусу та блокового виробництва. Celestia Core побудовано на [Tendermint Core](https://docs.tendermint.com/), модифікованому для зберігання коренів даних блоків із кодом стирання серед інших змін ([див. ADR](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Нода Celestia ([репозиторій](https://github.com/celestiaorg/celestia-node))

    * **Нода Celestia** доповнює вищезазначене за допомогою окремої мережі libp2p, яка обслуговує запити на вибірку доступності даних. Команда іноді називає це мережею «halo».

## Вимоги до обладнання

Для роботи мостової ноди є мінімальні вимоги до обладнання:

* Пам’ять: 8 GB RAM
* CPU: Quad-Core
* Розмір пам’яті на диску: 250 GB SSD
* Пропускна здатність: 1 Gbps для скачування/100 Mbps для загрузки

## Налаштування вашої мостової ноди

Цей туторіал виконано на машині Ubuntu Linux 20.04 (LTS) x64.

### Налаштування залежностей

Дотримуйтеся [цього](../developers/environment.md) туторіалу по установці залежностей.

## Запуск мостової ноди Celestia

### Установка ноди Celestia

Встановіть бінарний файл ноди Celestia, який використовуватиметься для запуску мостової ноди.

Дотримуйтеся туторіалу зі встановлення ноди Celestia [тут](../developers/celestia-node.md).

### Ініціалізація мостової ноди

Пропишіть наступне:

```sh
celestia bridge init --core.ip <ip-address> --core.rpc.port <port>
```

> NOTE: The `--core.rpc.port` defaults to 26657, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

Якщо вам потрібен список кінцевих точок RPC для підключення, ви можете побачити список [тут](./mamaki-testnet.md#rpc-endpoints)

### Запуск мостової ноди

Запустіть мостову ноду із підключенням до кінцевої точки gRPC ноди валідатора (яка зазвичай доступна на порту 9090):

```sh
celestia bridge start --core.ip <ip-address> --core.grpc.port <port>
```

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

Якщо вам потрібен список кінцевих точок RPC для підключення, ви можете побачити список [тут](./mamaki-testnet.md#rpc-endpoints)

Ви можете створити свій ключ для своєї ноди, дотримуючись інструкцій `cel-key` [тут](./keys.md)

Після запуску мостової ноди для вас буде згенеровано ключ гаманця. You will need to fund that address with Testnet tokens to pay for PayForData transactions. Ви можете знайти адресу, виконавши наступну команду:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

You have two networks to get testnet tokens from:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a bridge node for your validator it is highly recommended to request Mamaki testnet tokens as this is the testnet used to test out validator operations.

#### Додатково: запустіть мостову ноду за допомогою нетиповго ключа

Щоб запустити мостову ноду за допомогою нетипового ключа:

1. Нетиповий ключ має існувати в каталозі мостової ноди celestia за правильним шляхом (за замовчуванням: ~/.celestia-bridge/keys/keyring-test)
2. Ім'я нетипового ключа має бути передано під час `запуску`, наприклад:

```sh
celestia bridge start --core.ip <ip> --core.grpc.port 9090 --keyring.accname <name_of_custom_key>
```

### Додатково: запустіть мостову ноду за допомогою SystemD

Дотримуйтеся туторіала з налаштування мостової ноди як фонового процесу з SystemD [тут](./systemd.md#celestia-bridge-node).

Ви успішно налаштували мостову ноду, яка синхронізується з мережею.
