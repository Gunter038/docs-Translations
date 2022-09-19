---
sidebar_label: Configurar el entorno de red
---

# Configurando tu entorno para CosmWasm en Celestia
<!-- markdownlint-disable MD013 -->

Ahora el binario `wasmd` está compilado, necesitamos configurar una red local que comunique entre `wasmd` y Optimint.

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

Asegúratee de almacenar la salida de la wallet generada para referencia posterior si es necesario.

Ahora, vamos a añadir una cuenta de génesis y utilizarla para actualizar nuestro archivo de génesis:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Con eso, creamos un archivo de génesis de red local.

Algunos comandos más útiles que podemos configurar:

```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```

## Starting the Wasmd Network

We can run the following to start the `wasmd` network:

```sh
wasmd start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Please consider:

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Mamaki testnet tokens. Follow the tutorial for setting up a Celestia Light Node and creating a wallet with testnet faucet money [here](./node-tutorial.md) in the Celestia Node section.

Also please consider:

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Mamaki Testnet for `da_height`. You can find the latest block number in the explorer [here](https://testnet.mintscan.io/celestia-testnet). Also, for the flag `--optimint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

With that, we have kickstarted our `wasmd` network!
