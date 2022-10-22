---
sidebar_label: Interaction de Contrat
---

# Interaction de contrat avec Celestia sur CosmWasm
<!-- markdownlint-disable MD013 -->

Dans les étapes précédentes, nous avons stocké le hash de la transaction du contrat dans un environnement variable pour un usage ultérieur.

À cause des périodes plus longues de soumission des transactions via Rollmint en raison de l'attente de la couche de disponibilité des données de Celestia pour confirmer l'inclusion du bloc, nous devrons interroger directement notre hachage tx pour obtenir des informations à ce sujet.

## Interrogation du contrat

Commençons par questionner notre hash de transaction pour son code d'identification:

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

Cela va nous retourner le code d'identification du contrat déployé.

Dans notre cas, puisque c'est le premier contrat déployé sur notre réseau local, la valeur est `1`.

Maintenant, nous pouvons jeter un œil aux contrats générés par ce code d'identification :

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

Nous obtenons la sortie suivante :

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Instanciation du contrat

Nous commençons à instancier le contrat en écrivant le message `INIT` pour le contrat nameservice. Here, we are specifying that `purchase_price` of a name is `100uwasm` and `transfer_price` is `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Interaction avec le contrat

Maintenant que nous l'avons instancié, nous pouvons interagir davantage avec le contrat :

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

Cela nous permet de voir l'adresse du contrat, les détails de celui-ci et des soldes bancaires.

Maintenant, inscrivons un nom au contrat de notre adresse de portefeuille :

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

With that, we have instantiated and interacted with the CosmWasm nameservice smart contract using Celestia!
