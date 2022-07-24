- - -
sidebar_label : Commandes CLI utiles
- - -

# Commandes CLI utiles

Voir les options:

```console
$ celestia-appd --help
Start celestia app

Usage:
  celestia-appd [command]

Available Commands:
  add-genesis-account Add a genesis account to genesis.json
  collect-gentxs      Collect genesis txs and output a genesis.json file
  config              Create or query an application CLI configuration file
  debug               Tool for helping with debugging your application
  export              Export state to JSON
  gentx               Generate a genesis tx carrying a self delegation
  help                Help about any command
  init                Initialize private validator, p2p, genesis, 
  and application configuration files
  keys                Manage your application's keys
  migrate             Migrate genesis to a specified target version
  query               Querying subcommands
  rollback            rollback tendermint state by one height
  rollback            rollback cosmos-sdk and tendermint state by one height
  start               Run the full node
  status              Query remote node for status
  tendermint          Tendermint subcommands
  tx                  Transactions subcommands
  validate-genesis    validates the genesis file at the default 
  location or at the location passed as an arg
  version             Print the application binary version information
```

## Créer un wallet

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` configure le backend du trousseau de clés, où les clés sont stockées.

Les options sont: `os|file|kwallet|pass|test|memory`.

## Gestion des clés

```sh
# liste des clés
celestia-appd keys list

# ajouter des clés
celestia-appd keys add <KEY_NAME>

# supprimer des clés
celestia-appd keys delete <KEY_NAME>

# renommer des clés
celestia-appd keys rename <CURRENT_KEY_NAME> <NEW_KEY_NAME>
```

### Importation et exportation des clés

Importez une clé privée chiffrée et blindée ASCII dans la base de clés locale.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Exemple d'utilisation :

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Exporter une clé privée depuis le trousseau local au format chiffré et blindé ASCII :

```sh
celestia-appd keys export <KEY_NAME>

# vous serez alors invités à définir un mot de passe pour la clé privée chiffrée :
Entrez le mot de passe pour chiffrer la clé exportée :
```

Après avoir défini un mot de passe, votre clé chiffrée sera affichée.

## Interrogation des sous-commandes

Usage:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

Pour voir les options:

```sh
celestia-appd q --help
```

## Gestion des tokens

Obtenir le solde de tokens:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Exemple d'utilisation:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Transférer des tokens d'un portefeuille vers un autre:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Exemple d'utilisation:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

Pour voir les options:

```sh
celestia-appd tx bank send --help
```
