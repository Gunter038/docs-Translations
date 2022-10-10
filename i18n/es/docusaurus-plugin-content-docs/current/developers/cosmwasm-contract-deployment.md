---
sidebar_label: Despliegue del contrato
---

# Contract Deployment on CosmWasm with Rollmint
<!-- markdownlint-disable MD013 -->

## Compilar el Smart Contract

Ejecutaremos los siguientes comandos para descargar el smart contract de Nameservice y compilarlo:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

El contrato compilado es saldado a: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Pruebas unitarias

Si queremos ejecutar pruebas, podemos hacerlo con el siguiente comando:

```sh
cargo unit-test
```

## Contrato inteligente optimizado

Debido a que estamos desplegando el smart contract compilado a `wasmd`, queremos que sea lo más pequeño posible.

El equipo de CosmWasm proporciona una herramienta llamada `rust-optimizer` para la que necesitamos Docker para poder compilar.

Ejecuta el siguiente comando:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Esto colocará el bytecode Wasm optimizado en `artifacts/cw_nameservice.wasm`.

## Despliegue del contrato

¡Vamos a desplegar nuestro smart contract!

Ejecuta las siguientes instrucciones:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Esto le dará el hash de transacción para el despliegue de smart contract. Given we are using Rollmint, there will be a delay on the transaction being included due to Rollmint waiting on Celestia's Data Availability Layer to confirm the block has been included before submitting a new block.
