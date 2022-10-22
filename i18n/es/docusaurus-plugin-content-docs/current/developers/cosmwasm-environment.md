---
sidebar_label: Setup Network Environment
---

# Configurando tu entorno para CosmWasm en Celestia

Ahora que el binario `wasmd` está construido, necesitamos configurar una red local que se comunique entre `wasmd` y Optimint.

## Construyendo la red de Wasmd

Ejecuta el siguiente comando:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Esto inicializa una cadena llamada `celeswasm` con `wasmd` binario.

El siguiente comando nos ayuda a configurar cuentas para génesis:

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Asegúrese de almacenar el resultado del monedero generado para referencia posterior si es necesario.

Ahora, vamos a añadir una cuenta de génesis y utilizarla para actualizar nuestro archivo de génesis:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Con esto, hemos creado un archivo de génesis de red local.

Algunos comandos útiles más que podemos configurar:

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## Iniciando la red Wasmd

Podemos ejecutar lo siguiente para iniciar la red de `wasmd`:

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Ten en cuenta que:

> NOTA: En el comando anterior, necesitas pasar una dirección IP del nodo Celestia al `base_url` que tiene una cuenta con los tokens de la Devmet Arabica. Sigue el tutorial para configurar un Light Node de Celestia y crear una wallet con dinero de la faucet de testnet [aquí](./node-tutorial.md) en la sección de Celestia Node.

También ten en cuenta que:

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Arabica Devnet for `da_height`. You can find the latest block number in the explorer [here](https://explorer.celestia.observer/arabica). Also, for the flag `--rollmint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

With that, we have kickstarted our `wasmd` network!
