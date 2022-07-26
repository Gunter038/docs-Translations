- - -
sidebar_label : Light Node
- - -

# Configuration d'un Light Node Celestia

Ce tutoriel vous guidera à travers la mise en place d'un Light Node Celestia, qui vous permettra d'effectuer des échantillonnages de disponibilité de données sur le réseau de disponibilité de données (Data Availability/DA).

> Pour voir un tutoriel vidéo pour la mise en place d'un Light Node Celestia, cliquez [ici](../developers/light-node-video.md)

## Présentation des Light Nodes

Les Light nodes s'assurent de la disponibilité des données. C'est le moyen le plus commun d'interagir avec le réseau Celestia.

![light-node](/img/nodes/LightNodes.png)

Les Light Nodes ont le comportement suivant :

1. Ils écoutent les ExtendedHeaders, c'est-à-dire les en-têtes « bruts» enveloppés, qui notifient les nodes Celestia des nouveaux en-têtes de blocs et des métadonnées DA pertinentes.
2. Ils effectuent l'échantillonnage de la disponibilité des données (DAS) sur les en-têtes reçus.

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter un Light node :

* Mémoire: 2 Go de RAM
* CPU : Noyau unique
* Disque: 5 Go de stockage SSD
* Bande passante : 56 Gbps pour le téléchargement/56 Mbps pour l'upload

## Configuration de votre Light Node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Configurer les dépendances

Vous pouvez suivre le tutoriel pour configurer les dépendances [ici](../developers/environment.md).

## Installer Celestia Node

Suivez le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md)

### Initialiser le Light Node

Exécutez la commande suivante :

```sh
celestia light init
```

Vous devriez voir quelque chose comme :

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### Démarrer le Light Node

Démarre le Light node avec une connexion au point de terminaison gRPC d'un node de validateur (qui est généralement exposé sur le port 9090):

> REMARQUE : Pour avoir accès à la possibilité d'obtenir/de soumettre des informations relatives à l'état, telles que la possibilité de soumettre des transactions PayForData ou d'interroger solde de compte du node, un point de terminaison gRPC d'un Node Validateur (core) doit être transmis comme montré ci-dessous.

```sh
celestia light start --core.grpc http://<ip>:9090
```

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous démarrez le Light Node, une clé de wallet sera générée pour vous. Vous aurez besoin de financer cette adresse avec les jetons du Testnet Mamaki pour payer des transactions PayForData. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type light --keyring-backend test
```

Les tokens du Testnet Mamaki peuvent être demandés [ici](./mamaki-testnet.md#mamaki-testnet-faucet).

### Facultatif : Exécuter le Light Node avec une clé personnalisée

Afin d'exécuter un Light Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Light Node Celestia au bon chemin (default: `~/.celestia-light/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

```sh
celestia light start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Facultatif : Démarrer un Light Node avec SystemD

Suivez le tutoriel sur la configuration du Light Node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-light-node).

## Échantillonnage de la disponibilité des données (DAS)

Avec votre light node en cours d'exécution, vous pouvez consulter ce tutoriel sur la soumission de transactions `PayForData` [ici](../developers/node-tutorial.md).
