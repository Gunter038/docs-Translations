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
git checkout tags/v0.3.0
make install
```

Vérifiez que le binaire fonctionne et vérifiez la version avec la commande `celestia
version`:

```console
$ celestia version
Semantic version: v0.3.0
Commit: 00d80c423b2bfacec22a253ce6af3a534a1be3a7
```
