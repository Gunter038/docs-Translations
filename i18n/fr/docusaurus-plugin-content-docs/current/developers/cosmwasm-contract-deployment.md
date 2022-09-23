---
sidebar_label: Déploiement de contrat
---

# Déploiement de contrat sur CosmWasm avec Optimint
<!-- markdownlint-disable MD013 -->

## Compiler le Contrat Intelligent

We will run the following commands to pull down the Nameservice smart contract and compile it:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

The compiled contract is outputted to: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Unit Tests

Si nous voulons exécuter des essais, nous pouvons le faire par la commande suivante :

```sh
cargo unit-test
```

## Contrat Intelligent Optimisé

Because we are deploying the compiled smart contract to `wasmd`, we want it to be as small as possible.

CosmWasm team provides a tool called `rust-optimizer` which we need Docker for in order to compile.

Exécuter la commande suivante :

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

This will place the optimized Wasm bytecode at `artifacts/cw_nameservice.wasm`.

## Déploiement du Contrat

Déployons maintenant notre contrat intelligent !

Exécuter le code suivant :

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Cela vous fournira le hash de la transaction pour le déploiement du contrat intelligent. Given we are using Optimint, there will be a delay on the transaction being included due to Optimint waiting on Celestia's Data Availability Layer to confirm the block has been included before submitting a new block.
