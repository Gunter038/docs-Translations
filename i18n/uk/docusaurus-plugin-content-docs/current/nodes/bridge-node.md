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
celestia bridge init --core.remote tcp://<ip-address>:26657
```

Якщо вам потрібен список кінцевих точок RPC для підключення, ви можете побачити список [тут](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

Start the Bridge Node with a connection to a validator node's gRPC endpoint (which is usually exposed on port 9090):

> NOTE: In order for access to the ability to get/submit state-related information, such as the ability to submit PayForData transactions, or query for the node's account balance, a gRPC endpoint of a validator (core) node must be passed as directed below._

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

You can create your key for your node by following the `cel-key` instructions [here](./keys.md)

Once you start the Bridge Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Mamaki Testnet tokens can be requested [here](./mamaki-testnet.md#mamaki-testnet-faucet).

#### Optional: run the bridge node with a custom key

In order to run a bridge node using a custom key:

1. The custom key must exist inside the celestia bridge node directory at the correct path (default: `~/.celestia-bridge/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: start the bridge node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.
