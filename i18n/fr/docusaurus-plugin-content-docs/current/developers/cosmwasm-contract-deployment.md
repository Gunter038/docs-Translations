---
sidebar_label: Déploiement contractuel
---

# Déploiement contractuel de cosmwasm avec rollmint
<!-- markdownlint-disable MD013 -->

## Écrire un contrat intelligent

Nous allons exécuter les commandes suivantes pour retirer le contrat intelligent Nameservice et le compiler :

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Le contrat compilé est affiché à: `cible/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Tests unitaires

Si nous voulons exécuter des essais, nous pouvons le faire par la commande suivante :

```sh
cargo unit-test
```

## Contrat Intelligent Optimisé

Parce que nous déployons le contrat intelligent compilé sur `wasmd`, nous voulons qu'il soit aussi petit que possible.

L'équipe CosmWasm fournit un outil appelé `rust-optimizer` qui a besoin de Docker pour pouvoir compiler.

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

Cela vous fournira le hash de la transaction pour le déploiement du contrat intelligent. Étant donné que nous utilisons Rollmint, il y aura un retard dans l'inclusion de la transaction car Rollmint attend sur la couche de disponibilité des données de Celestia pour confirmer que le bloc a été inclus avant de soumettre un nouveau bloc.
