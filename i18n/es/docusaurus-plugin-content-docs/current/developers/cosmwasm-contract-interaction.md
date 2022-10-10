---
sidebar_label: Contrato de interacción
---

# Contrato de interacción en CosmWasm con Celestia
<!-- markdownlint-disable MD013 -->

En los pasos anteriores, hemos almacenado el hash tx del contrato en una variable de entorno para su uso posterior.

Because of the longer time periods of submitting transactions via Rollmint due to waiting on Celestia's Data Availability Layer to confirm block inclusion, we will need to query our  tx hash directly to get information about it.

## Consulta de Contrato

Empecemos por consultar nuestro hash de transacción para su ID de código:

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

Esto nos devolverá el Código ID del contrato desplegado.

En nuestro caso, ya que es el primer contrato implementado en nuestra red local, el valor es `1`.

Ahora, podemos echar un vistazo a los contratos instanciados por este código ID:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

Obtenemos la siguiente salida:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Contrato Instanciación

Empezamos a instanciar el contrato escribiendo el siguiente `mensaje` INIT para el contrato nameservice. Aquí, especificamos que `purchase_price` de un nombre es `100uwasm` y `transfer_price` es `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Contrato de interacción

Ahora que lo instanciamos, podemos interactuar más con el contrato:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

Esto nos permite ver la dirección del contrato, los detalles del contrato y saldos bancarios.

Ahora, vamos a registrar un nombre al contrato para nuestra dirección de wallet:

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

¡Con eso, hemos instanciado e interactuado con el smart contract de nameservice de CosmWasm usando Celestia!
