- - -
sidebar_label : Installer Celestia Node
- - -

# Celestia Node

Ce tutoriel passe en revue l'installation de celestia-node. Ce tutoriel suppose que vous avez terminé les étapes de configuration de votre propre environnement [ici](./environment.md).

## Installer Celestia Node

### Installation d'Arabica

Installer un node Celestia pour le Devnet Arabica signifie installer une version spécifique compatible avec le réseau.

Installez le binaire Celestia-node en exécutant les commandes suivantes :

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Vérifiez que le binaire fonctionne et vérifiez la version avec la commande `celestia
version`:

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Installation de Mamaki

Installer un node Celestia pour le Testnet Mamaki signifie installer une version spécifique compatible avec le réseau.

Installer le fichier binaire de celestia-node en exécutant les commandes suivantes :

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Vérifiez que le fichier binaire fonctionne et la version avec la commande `celestia
version` :

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Sélection du réseau

Vous pouvez sélectionner un réseau dans celestia-node entre Arabica et Mamaki. Cependant, vous noterez que les réseaux fonctionnent mieux sur les versions de celestia-node mentionnées ci-dessus.

```sh
celestia light init
celestia light start --node.network arabica
```
