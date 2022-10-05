- - -
sidebar_label : Clés
- - -

# Prise en main de l'utilitaire Cel-Key

A l'intérieur du dépôt du nœud Celestia se trouve un utilitaire appelé `cel-key` qui utilise l'utilitaire de clé fourni par le Cosmos-SDK et sa technologie. L'utilitaire peut être pour `add`(ajouter), `delete`(effacer) , et gérer les clés pour tout type de node de Disponilité de la donnée `(bridge || full || light)`, ou juste des clés en général.

## Installation

Vous devez d'abord "pull down" le dépôt `celestia-node`:

```sh
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

Il peut être construit en utilisant l'une des commandes suivantes :

```sh
# dumps binary in current working directory, accessible via `./cel-key`
make cel-key
```

ou

```sh
# installs binary in GOBIN path, accessible via `cel-key`
make install-key
```

Dans le cadre de ce guide, nous utiliserons la commande `make cel-key`.

## Étapes pour générer une clé de **bridge** node

Pour générer une clé pour un bridge node, procédez comme ce qui suit :

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

Cela chargera la clé <key_name> dans le répertoire du Bridge node.

## Étapes pour générer une clé de **full** node

Pour générer une clé pour un full node Celestia, procédez comme suit :

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

Cela chargera la clé <key_name> dans le répertoire du full node.

## Étapes pour générer une clé de **light** node

Pour générer une clé pour un light node Celestia, procédez comme suit :

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

Cela chargera la clé <key_name> dans le répertoire du light node.

## Étapes pour générer des clés de **light** node

Vous pouvez exporter une clé privée à partir du trousseau local au format crypté et ASCII-armored.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

Vous pouvez ensuite importer votre clé avec `celestia-appd`:

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
