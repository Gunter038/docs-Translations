---
sidebar_label: Взаимодействие с контрактом
---

# Взаимодействие с контрактом на CosmWasm с помощью Celestia
<!-- markdownlint-disable MD013 -->

В предыдущих шагах мы сохранили хеш tx контракта как переменную для последующего использования.

Because of the longer time periods of submitting transactions via Rollmint due to waiting on Celestia's Data Availability Layer to confirm block inclusion, we will need to query our  tx hash directly to get information about it.

## Запрос контракта

Давайте начнем с запроса хеша нашей транзакции для код ID:

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

Это даст нам код ID развернутого контракта.

В нашем случае, поскольку это первый контракт, развернутый в нашей локальной сети, значение равно `1`.

Теперь мы можем взглянуть на контракты, созданные под этим код ID:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

Мы получаем следующий вывод:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Заключение контракта

Мы начинаем выполнение контракта, написав следующее сообщение `INIT` для контракта службы имен. Здесь мы указываем, что `purchase_price` имени составляет `100uwasm`, а `transfer_price` - `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Взаимодействие с контрактом

Теперь, когда мы запустили контракт, мы можем взаимодействовать с ним дальше:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

Это позволяет нам увидеть адрес контракта, детали контракта и баланс.

Теперь давайте запишем в контракте имя для адреса нашего кошелька:

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Запрос владельца записи имени
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

Таким образом, мы запустили и начали взаимодействовать с смарт контрактом сервис имен CosmWasm с помощью Celestia!
