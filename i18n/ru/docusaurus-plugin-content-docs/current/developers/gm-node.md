---
sidebar_label: Запуск узла Light
---

# Запуск узла Celestia DA Light

Для выполнения этого руководства требуется узел Celestia Light в сети Mamaki Testnet. Выполните следующие команды для установки узла Celestia:

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

Внутри репозитория celestia-node находится утилита `cel-key`, которая использует ключевую утилиту, предоставляемую Cosmos-SDK под капотом. Утилита может быть использована для команд `add`, `delete`, и управлять ключами для любого типа узла DA `(bridge || full || light)`, или просто ключами в целом.

## 🗝 Создайте ключ

Создайте Ваш ключ для узла:

```bash
<code>make cel-key<code>
```

Verify the version of your Celestia-Node with the `celestia version` command, it should be `v0.3.0-rc2`:

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

## 🟢 Initialize Light Node

Now, we’re ready to initialize the Celestia Light Node. You can do so by running:

```bash
celestia light init
```

Query our key's address using `cel-key` :

```bash
./cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 🚰 Visit Faucet

Use the `#mamaki-faucet` channel in the Celestia Discord to request testnet tokens:

```bash
$request <Wallet-Address>
```

Start Celestia Light node with a connection to a public Core Endpoint:

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![3.png](/img/gm/3.png)

In another terminal window, check the balance from our visit to the faucet:

```bash
curl -X GET http://localhost:26658/balance
```

Your response should look like this, denominated in `utia` in JSON format.

```bash
{"denom":"utia","amount":"100000000"}
```

Now that we are set with Go and Ignite CLI installed, and our Celestia Light Node running on our machine, we’re ready to build, test, and launch our own sovereign rollup.
