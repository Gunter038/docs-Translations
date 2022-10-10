---
sidebar_label: Configurer l'Environnement du Réseau
---

# Configurer votre environnement pour CosmWasm sur Celestia

Now the `wasmd` binary is built, we need to setup a local network that communicates between `wasmd` and Rollmint.

## Construire le réseau Wasmd

Exécuter la commande suivante :

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Cela initialise une chaine appelée `celeswasm` avec le fichier binaire `wasmd`.

La commande suivante nous aide à configurer des comptes pour le genesis :

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Assurez-vous de stocker le produit du portefeuille généré pour un usage ultérieur si besoin.

Maintenant, ajoutons un compte genesis et utilisons-le pour mettre à jour notre fichier genesis:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Grâce à cela, nous avons créé un fichier genesis sur le réseau local.

D'autres commandes encore plus utiles que nous pouvons configurer :

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## Lancer le réseau Wasmd

Nous pouvons exécuter la commande suivante pour lancer le réseau `wasmd` :

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Remarques:

> NOTE : Dans la commande du dessus, vous avez besoin de fournir une adresse IP d'un node Celestia possédant des Tokens du Devnet Arabica à `base_url`. Suivre le tutoriel pour configurer un Light Node Celestia et créer un wallet avec les tokens faucet de testnet dans la section node de Celestia [ici](./node-tutorial.md).

Remarques complémentaires :

> IMPORTANT : Egalement dans la commande d'au dessus, vous avez besoin de spécifier la dernière position du bloc dans le devnet Arabica par `da_height`. Vous pouvez trouver le numéro du dernier bloc dans l'explorateur [ici](https://explorer.celestia.observer/arabica). Also, for the flag `--rollmint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

Grâce à cet article, nous avons démarré notre réseau `wasmd` !
