---
sidebar_label: Запуск Лайт Ноды
---

# Запуск Лайт Ноды Celestia DA

Для выполнения этого руководства требуется запустить Лайт Ноду Celestia в сети Mamaki Testnet. Выполните следующие команды для установки ноды Celestia:

<!-- markdownlint-disable MD010 -->
```bash
cd && rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node
git checkout tags/v0.3.0-rc2
make install
```
<!-- markdownlint-enable MD010 -->

![1.png](/img/gm/1.png)

Внутри репозитория celestia-node находится утилита `cel-key`, которая использует ключевую утилиту, предоставляемую Cosmos-SDK под капотом. Утилита может быть использована для функций `add`, `delete`, а также управлять ключами любого типа ноды DA `(bridge || full || light)`, или просто ключами в целом.

## 🗝 Создайте ключ

Создайте Ваш ключ для ноды:

```bash
<code>make cel-key<code>
```

Проверьте версию вашей ноды Celestia с помощью функции `celestia version`, она должна быть `v0.3.0-rc2`:

```bash
celestia version
```

```bash
# OUTPUT

#Semantic version: v0.3.0-rc2
#Commit: 89892d8b96660e334741987d84546c36f0996fbe
#Build Date: Fri Oct  7 01:08:14 UTC 2022
#System version: amd64/linux
#Golang version: go1.18.2
```

## 🟢 Инициализация Лайт Ноды

Теперь мы готовы инициализировать Лайт Ноду Celestia. Вы можете сделать это, запустив:

```bash
celestia light init
```

Запросите адрес Вашего ключа с помощью функции `cel-key`:

```bash
./cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 🚰 Посетите Faucet

Используйте канал `#mamaki-faucet` в Discord для запроса токенов в тестовой сети:

```bash
$request <Wallet-Address>
```

Запустите Лайт Ноду Celestia с подключением к общественной конечной точке Core Endpoint:

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![31.png](/img/gm/3.png)

В другом окне терминала, проверьте баланс после использования Faucet:

```bash
curl -X GET http://127.0.0.1:26658/balance
```

Ваш ответ должен выглядеть следующим образом, обозначенный как `utia` в формате JSON.

```bash
{"denom":"utia","amount":"100000000"}
```

Теперь, когда у нас установлены Go и Ignite CLI, а наша Лайт Нода Celestia запущена, мы готовы к созданию, тестированию и запуску нашего собственного роллапа.
