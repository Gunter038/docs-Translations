- - -
sidebar_label : Full Storage Node
- - -

# Configuration d'un Full Storage Node Celestia

Ce tutoriel vous guidera à travers la mise en place d'un Full Storage Node Celestia, qui est un node Celestia qui ne se connecte pas à Celestia App (donc pas un full node) mais stocke toutes les données.

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le full storage node:

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configuration de votre Full Storage Node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Configurer les dépendances

Vous pouvez suivre le tutoriel pour configurer les dépendances [ici](../developers/environment.md)

## Installer Celestia node

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

Une note sur les ports :

> NOTE : Le port `--core.grpc.port` est configuré par défaut à 9090, donc si vous n'en spécifiez pas un autre dans la ligne de commande, il s'exécutera sur celui-là par défaut. Vous pouvez utiliser le drapeau (flag) pour spécifier un autre port si vous préférez.

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port>
```
<!-- markdownlint-enable MD013 -->

Si vous souhaitez trouver des exemples de points de terminaison RPC, consultez la liste des ressources [ici](./mamaki-testnet.md#rpc-endpoints).

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous démarrez le Node, une clé de wallet sera générée pour vous. Vous aurez besoin de financer cette adresse avec les jetons du Testnet Mamaki pour payer des transactions PayForData. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type full --keyring-backend test
```

Vous avez le choix entre deux réseaux pour obtenir des jetons de testnet :

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE : Si vous souhaitez lancer un full-storage node pour votre rollup souverain, il est hautement recommandé de demander des jetons du devnet Arabica car c'est le devnet qui reçoit les dernières mises à jour utiles au développement de votre rollup souverain. Vous pouvez également utiliser le testnet Mamaki, bien qu'il soit davantage conçu pour les opérations de Validateurs.

### Facultatif : Exécuter le Full Storage Node avec une clé personnalisée

Afin d'exécuter un Full Storage Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Full Storage Node Celestia au bon chemin (default: `~/.celestia-full/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port> --keyring.accname <name-of-custom-key>
```
<!-- markdownlint-enable MD013 -->

### Facultatif : Démarrez le Full Storage Node avec SystemD

Suivez le tutoriel sur la configuration du full storage node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-full-storage-node).

Avec cela, vous exécutez maintenant Full Storage Node Celestia.
