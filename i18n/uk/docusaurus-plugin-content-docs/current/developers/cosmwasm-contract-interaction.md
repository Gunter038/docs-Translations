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

Тепер ми можемо поглянути на контракти, здійснені за допомогою ID Коду:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

Ми отримаємо такий вивід:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Створення екземпляра контракту

Ми починаємо створення екземпляра контракту, написавши наступне повідомлення `INIT` для контракту служби імен. Тут ми вказуємо, що `purchase_price` імені становить `100uwasm`, а `transfer_price` — `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Контрактна взаємодія

Тепер, коли ми створили контракт, можна далі взаємодіяти з ним:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

Це дозволяє нам бачити адресу контракту, деталі контракту та банківські баланси.

Тепер зареєструймо ім'я контракту для адреси нашого гаманця:

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

Таким чином, ми створили екземпляр смартконтракту служби імен CosmWasm і взаємодіяли з ним за допомогою Celestia!
