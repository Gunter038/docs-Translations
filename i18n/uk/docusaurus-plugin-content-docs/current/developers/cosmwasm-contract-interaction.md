---
sidebar_label: Contract Interaction
---

# Контрактна взаємодія на CosmWasm із Celestia
<!-- markdownlint-disable MD013 -->

У попередніх кроках ми зберегли хеш контракту tx у змінній середовища для подальшого використання.

Через триваліші періоди надсилання транзакцій через Rollmint через очікування на рівні доступності даних Celestia для підтвердження включення блоку, нам потрібно буде запитати напряму наш хеш передачі, щоб отримати інформацію про це.

## Контрактний запит

Розпочнімо з запиту хешу нашої транзакції для свого коду:

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

Ми повернемося до ID коду розгорнутого контракту.

У нашому випадку, оскільки це перший контракт, розгорнутий у нашій локальній мережі, значення дорівнює `1`.

Now, we can take a look at the contracts instantiated by this Code ID:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

We get the following output:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Contract Instantiation

We start instantiating the contract by writing up the following `INIT` message for nameservice contract. Here, we are specifying that `purchase_price` of a name is `100uwasm` and `transfer_price` is `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Contract Interaction

Now that we instantiated it, we can interact further with the contract:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

This allows us to see the contract address, contract details, and bank balances.

Now, let's register a name to the contract for our wallet address:

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

With that, we have instantiated and interacted with the CosmWasm nameservice smart contract using Celestia!
