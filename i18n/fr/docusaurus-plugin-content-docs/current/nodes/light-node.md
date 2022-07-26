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

> NOTE: In order for access to the ability to get/submit state-related information, such as the ability to submit PayForData transactions, or query for the node's account balance, a gRPC endpoint of a validator (core) node must be passed as directed below.

```sh
celestia light start --core.grpc http://<ip>:9090
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

You can create your key for your node by following the `cel-key` instructions [here](./keys.md)

Once you start the Light Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command:

```sh
./cel-key list --node.type light --keyring-backend test
```

Mamaki Testnet tokens can be requested [here](./mamaki-testnet.md#mamaki-testnet-faucet).

### Optional: Run the Light Node with a Custom Key

In order to run a light node using a custom key:

1. The custom key must exist inside the celestia light node directory at the correct path (default: `~/.celestia-light/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia light start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: Start Light Node with SystemD

Follow the tutorial on setting up the light node as a background process with SystemD [here](./systemd.md#celestia-light-node).

## Data Availability Sampling (DAS)

With your light node running, you can check out this tutorial on submitting `PayForData` transactions [here](../developers/node-tutorial.md).
