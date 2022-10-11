---
sidebar_label: 合约交互
---

# CosmWasm 与 Celestia 的合约交互
<!-- markdownlint-disable MD013 -->

在前面的步骤中，我们将合约的 tx 哈希存储在一个环境变量里供以后使用。

由于通过Rollmint提交交易的时间较长 由于等待Celestia的数据可用性层确认块包含， 我们需要直接查询tx散列以获取有关它的信息。

## 合约查询

让我们先查询交易哈希代码ID：

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

这将为我们返回已部署合约的代码 ID。

在我们的例子中，因为它是部署在我们本地网络上的第一个合约所以它的值为`1`。现在我们可以看看这个代码 Id 实例化的合约：

现在，我们可以看看这个代码 ID 实例化的合约：

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

我们得到以下输出：

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## 合约实例化

我们通过为 nameservice 合约编写以下 `INIT` 消息来开始实例化合约。 这里，我们对 `purchase_price` 和 `transfer_price` 分别赋值 `100uwasm` 和 `999uwasm`。

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

这样，我们就已经使用 Celestia 实例化了 CosmWasm 名称服务并与智能合约交互！
