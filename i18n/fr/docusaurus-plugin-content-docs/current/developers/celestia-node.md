- - -
sidebar_label : Installing Celestia Node
- - -

# Celestia Node

Ce tutoriel passe en revue l'installation de Celestia-Node. Ce tutoriel suppose que vous avez terminé les étapes de configuration de votre propre environnement [ici](./environment.md).

## Installer Celestia Node

Installez le binaire Celestia-node en exécutant les commandes suivantes :

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Vérifiez que le binaire fonctionne et vérifiez la version avec la commande `celestia
version`:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```
