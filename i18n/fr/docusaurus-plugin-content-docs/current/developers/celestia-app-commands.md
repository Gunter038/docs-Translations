- - -
sidebar_label : Commandes CLI utiles
- - -

# Commandes CLI utiles

Voir les options:

```console
$ celestia-appd --help
Start celestia app

Utilisation:
  celestia-appd [command]

Commandes disponibles:
  add-genesis-account Ajoute un compte genesis à genesis.json
  collect-gentxs      Collecte les transactions genesis et produit un fichier genesis.json
  config              Crée ou demande un fichier de configuration pour une application CLI
  debug               Outil d'aide au débogage de votre application
  export              Exporte l'état en JSON
  gentx               Génère une transaction genesis portant une auto-délégation 
  help                Aide pour toute commande 
  init                Initialise un validateur privé, pair-à-pair, genesis, et les fichiers de configuration de l'application 
  keys                Gérer les clés de votre application
  migrate             Migrer genesis vers une destination spécifique
  query               Requête de sous-commandes
  rollback            Retourne l'état Tendermint d'une grandeur en arrière 
  rollback            Retourne les états cosmos-sdk etTendermint d'une grandeur en arrière 
  start               Exécute le full node 
  status              Envoie une demande de statut au noeud éloigné
  tendermint          Sous-commandes Tendermint 
  tx                  Sous-commandes de Transactions 
  validate-genesis    valide le fichier genesis à l'emplacement par défaut ou à l'emplacement spécifié en arg 
  version             Imprime la version de l'information binaire de l'application
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

Importez une clé privée chiffrée et ASCII-armored dans la base de clés locale.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Exemple d'utilisation :

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Exporter une clé privée à partir du trousseau local au format crypté et ASCII-armored:

```sh
celestia-appd keys export <KEY_NAME>

# vous serez alors invités à définir un mot de passe pour la clé privée chiffrée :
Entrez la passphrase pour chiffrer la clé exportée :
```

Après avoir défini un mot de passe, votre clé chiffrée sera affichée.

## Interrogation des sous-commandes

Utilisation:

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

Transférer des tokens d'un wallet vers un autre:

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

## Gouvernance

Vous pouvez voter lors d'une proposition de gouvernance avec la commande suivante :

```sh
celestia-appd tx gov vote <proposal id> <yes or no> --from <wallet> --chain-id <chain-id>
```

## Réclamer les récompenses de validateur

Vous pouvez réclamer vos récompenses de validateur avec la commande suivante :

```sh
celestia-appd tx distribution withdraw-rewards <validator valoper>\
    --commission --from=<validator wallet> --chain-id <chain-id> --gas auto -y
```

## Déléguer et retirer la délégation de ses jetons

Vous pouvez `delegate` (déléguer) vos tokens à un validateur avec la commande suivante :

```sh
celestia-appd tx staking delegate <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

Vous pouvez dé-déléguer des jetons à un validateur avec la commande `unbond`:

```sh
celestia-appd tx staking unbond <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

## Unjail le validateur

Vous pouvez "unjail" votre validateur avec la commande suivante :

```sh
celestia-appd tx slashing unjail --from <validator wallet>\
    --chain-id <chain-id> --gas auto -y
```

## Comment exporter les logs avec SystemD

Vous pouvez exporter vos logs si vous exécutez un service SystemD avec la commande suivante :

```sh
sudo journalctl -u <your systemd service> -S yesterday > node_logs.txt
sudo journalctl -u <your systemd service> -S today > node_logs.txt
# This command outputs the last 1 million lines!
sudo journalctl -u <your systemd service> -n 1000000 > node_logs.txt
```
