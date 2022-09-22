- - -
sidebar_label : Full Storage Node
- - -

# Setting up a Celestia Full Storage Node

Ce tutoriel vous guidera à travers la mise en place d'un Full Storage Node Celestia, qui est un node Celestia qui ne se connecte pas à Celestia App (donc pas un full node) mais stocke toutes les données.

## Hardware requirements

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le full storage node :

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Setting up your full storage node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Setup the dependencies

Vous pouvez suivre le tutoriel pour configurer les dépendances [ici](../developers/environment.md)

## Install Celestia node

> Remarque : Assurez-vous que vous disposez d'au moins 250 Go d'espace libre pour un Full Storage Node Celestia

Vous pouvez suivre le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md)

### Run the full storage node

#### Initialize the full storage node

Exécutez la commande suivante :

```sh
celestia full init
```

#### Start the full storage node

Démarrez le Full storage node avec une connexion au point de terminaison gRPC d'un validator node (qui est généralement exposé sur le port 9090):

> REMARQUE : Pour avoir accès à la possibilité d'obtenir/soumettre des informations, telles que la possibilité de soumettre des transactions PayForData, ou interroger le solde du compte du Node, un point de terminaison gRPC d'un validateur (core) doit être transmis comme indiqué ci-dessous.

A note on ports:

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port>
```
<!-- markdownlint-enable MD013 -->

Si vous souhaitez trouver des exemples de points de terminaison RPC, consultez la liste des ressources [ici](./mamaki-testnet.md#rpc-endpoints).

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous démarrez le Node, une clé de wallet sera générée pour vous. You will need to fund that address with testnet tokens to pay for PayForData transactions. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type full --keyring-backend test
```

You have two networks to get testnet tokens from:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a full-storage node for your sovereign rollup, it is highly recommended to request Arabica devnet tokens as Arabica has the latest changes that can be used to test for developing your sovereign rollup. You can still use Mamaki Testnet as well, it is just mostly used for Validator operations.

### Optional: run the full storage node with a custom key

Afin d'exécuter un Full Storage Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Full Storage Node Celestia au bon chemin (default: `~/.celestia-full/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

<!-- markdownlint-disable MD013 -->
```sh
celestia full start --core.ip http://<ip-address> --core.grpc.port <port> --keyring.accname <name-of-custom-key>
```
<!-- markdownlint-enable MD013 -->

### Optional: start the full storage node with SystemD

Suivez le tutoriel sur la configuration du full storage node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-full-storage-node).

Avec cela, vous exécutez maintenant Full Storage Node Celestia.
