---
sidebar_label: Despliegue de contratos
---

# Despliegue de contratos en CosmWasm con Rollmint
<!-- markdownlint-disable MD013 -->

## Compilar el Smart Contract

Ejecutaremos los siguientes comandos para descargar el smart contract de Nameservice y compilarlo:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

El contrato compilado se emite en: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Pruebas unitarias

Si queremos ejecutar pruebas, podemos hacerlo con el siguiente comando:

```sh
cargo unit-test
```

## Smart Contract optimizado

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

Esto te dará el hash de transacción para el despliegue de smart contract. Dado que estamos usando Rollmint, habrá un retraso en la transacción que se incluye debido a la espera de Rollmint en la capa de disponibilidad de datos de Celestia para confirmar que el bloque ha sido incluido antes de enviar un nuevo bloque.
