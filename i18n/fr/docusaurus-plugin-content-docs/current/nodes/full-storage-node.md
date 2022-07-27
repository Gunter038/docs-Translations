- - -
sidebar_label : Full Storage Node
- - -

# Configuration d'un Full Storage Node Celestia

Ce tutoriel vous guidera à travers la mise en place d'un Full Storage Node Celestia, qui est un node Celestia qui ne se connecte pas à Celestia App (donc pas un full node) mais stocke toutes les données.

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le full storage node :

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configuration de votre Full Storage Node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Configurer les dépendances

Vous pouvez suivre le tutoriel pour configurer les dépendances [ici](../developers/environment.md)

## Installer Celestia Node

> Remarque : Assurez-vous que vous disposez d'au moins 250 Go d'espace libre pour un Full Storage Node Celestia

Vous pouvez suivre le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md)

### Démarrer le Full Storage Node

#### Initialiser le Full Storage Node

Exécutez la commande suivante :

```sh
celestia full init
```

#### Démarrer le Full Storage Node

Démarrez le Full storage node avec une connexion au point de terminaison gRPC d'un validator node (qui est généralement exposé sur le port 9090):

> REMARQUE : Pour avoir accès à la possibilité d'obtenir/soumettre des informations, telles que la possibilité de soumettre des transactions PayForData, ou interroger le solde du compte du Node, un point de terminaison gRPC d'un validateur (core) doit être transmis comme indiqué ci-dessous.

```sh
celestia full start --core.grpc http://<ip addr of core node>:9090
```

Si vous souhaitez trouver des exemples de points de terminaison RPC, consultez la liste des ressources [ici](./mamaki-testnet.md#rpc-endpoints).

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous démarrez le Node, une clé de wallet sera générée pour vous. Vous aurez besoin de financer cette adresse avec les jetons du Testnet Mamaki pour payer transactions PayForData. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type full --keyring-backend test
```

Les tokens du Testnet Mamaki peuvent être demandés [ici](./mamaki-testnet.md#mamaki-testnet-faucet).

### Facultatif : Exécuter le Full Storage Node avec une clé personnalisée

Afin d'exécuter un Full Storage Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Full Storage Node Celestia au bon chemin (default: `~/.celestia-full/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

```sh
celestia full start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Facultatif : Démarrez le Full Storage Node avec SystemD

Suivez le tutoriel sur la configuration du full storage node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-full-storage-node).

Avec cela, vous exécutez maintenant Full Storage Node Celestia.
