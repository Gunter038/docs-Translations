---
sidebar_label: 合约交互
---

# CosmWasm 与 Celestia 的合约交互
<!-- markdownlint-disable MD013 -->

在前面的步骤中，我们将合约的 tx 哈希存储在环境变量中以供以后使用。

由于等待 Celestia 的数据可用性层确认区块包含，通过 Rollmint 提交交易的时间较长，我们需要直接查询我们的 tx 哈希以获取有关它的信息。

## 合约查询

让我们先查询我们的交易哈希代码ID：

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

这将为我们返回已经部署合约好的代码 ID。

在我们的例子中，因为它是部署在我们本地网络上的第一个合约，所以它的值为`1`。

现在，我们可以看看这个代码 ID 实例化的合约：

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

我们得到以下输出：

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## 合约实例化

我们通过以下 `INIT` 消息 作为命名服务合约的实例来开始执行合约。 在这里，我们对 `purchase_price`分别赋值 `100uwasm`和 `transfer_price`是 `999uwasm`。

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## 合约交互

现在我们实例化了它，我们可以进一步与合约交互：

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

这使我们能够查看合约地址、合约详细信息以及钱包余额。

现在，让我们为我们的钱包地址注册一个名称：

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

这样，我们就已经使用 Celestia 实例化了 CosmWasm 命名服务智能合约并与其进行交互！
