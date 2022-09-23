---
sidebar_label: Interaction de Contrat
---

# Interaction de contrat avec Celestia sur CosmWasm
<!-- markdownlint-disable MD013 -->

Dans les étapes précédentes, nous avons stocké le hash de la transaction du contrat dans un environnement variable pour un usage ultérieur.

Because of the longer time periods of submitting transactions via Optimint due to waiting on Celestia's Data Availability Layer to confirm block inclusion, we will need to query our  tx hash directly to get information about it.

## Contract Querying

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

We get the following output:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Contract Instantiation

We start instantiating the contract by writing up the following `INIT` message for nameservice contract. Here, we are specifying that `purchase_price` of a name is `100uwasm` and `transfer_price` is `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Contract Interaction

Now that we instantiated it, we can interact further with the contract:

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

Grâce à cela, nous avons généré des exemples et interagi avec le contrat intelligent du nom de domaine CosmWasm en utilisant Celestia !
