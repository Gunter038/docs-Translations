- - -
sidebar_label : Bridge Node
- - -

# Setting up a Celestia Bridge Node

Ce tutoriel passera en revue les étapes de configuration de votre Bridge node Celestia (Noeud Pont).

Les Bridge nodes connectent la couche de disponibilité des données et la couche de consensus tout en offrant la possibilité de devenir validateur.

Validators do not have to run bridge nodes, but are encouraged to in order to relay blocks to the data availability network.

## Overview of bridge nodes

Un Bridge node Celestia a les propriétés suivantes:

1. Importez et traitez les en-têtes et blocs "bruts" à partir d'un processus Core de confiance (c'est-à-dire une connexion RPC de confiance à un node celestia-core) dans le Réseau consensuel. Les bridge nodes peuvent exécuter ce processus principal en interne (intégré) ou se connecter simplement à un terminal distant. Les bridge nodes offrent aussi la possibilité d'être un validateur actif dans le consensus du réseau .
2. Valider et effacer le code des blocs « bruts »
3. Fournir des parts de blocs avec des en-têtes de disponibilité des données aux Light Nodes du réseau DA (Data availabilty, disponibilité des données). ![bridge-node-diagram](/img/nodes/BridgeNodes.png)

Du point de vue de l'implémentation, les Bridge nodes exécutent deux processus distincts:

1. Celestia App avec Celestia Core ([voir le repo](https://github.com/celestiaorg/celestia-app))

    * **Celestia App** est la machine où l'application et la logique de Proof-of-Stake (Preuve d'enjeu) sont exécutées. L'application Celestia est construite sur le [Cosmos SDK](https://docs.cosmos.network/) et englobe également **Celestia Core**.
    * **Celestia Core** est l'interaction d'état, le consensus et la couche de production de blocs. Celestia Core est construite sur [Tendermint Core](https://docs.tendermint.com/), modifié pour stocker des racines de données d'erasure block coding parmi d'autres changements ([voir ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia Node ([voir le repo](https://github.com/celestiaorg/celestia-node))

    * **Celestia Node** augmente ce qui précède avec un réseau libp2p séparé qui sert des demandes d'échantillonnage de la disponibilité des données. L'équipe appelle parfois ceci le réseau « halo ».

## Hardware requirements

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le Bridge node :

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Setting up your bridge node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Setup the dependencies

Suivez le tutoriel ici pour installer les dépendances [ici](../developers/environment.md).

## Deploy the Celestia bridge node

### Install Celestia node

Installez le binaire Celestia Node, qui sera utilisé pour exécuter le Bridge node.

Suivez le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md).

### Initialize the bridge node

Exécutez les commandes suivantes:

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657
```

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

Start the Bridge Node with a connection to a validator node's gRPC endpoint (which is usually exposed on port 9090):

> REMARQUE : Pour accéder à la possibilité d'obtenir/de soumettre des informations relatives à l'état, telles que la possibilité de soumettre des transactions PayForData ou d'interroger solde du compte du node, un point de terminaison gRPC d'un node validateur doit être transmis comme ci-dessous

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous aurez démarré le Bridge Node, une clé de wallet sera générée pour vous. Vous aurez besoin de financer cette adresse avec les jetons du Testnet Mamaki pour payer transactions PayForData. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Les tokens du Testnet Mamaki peuvent être demandés [ici](./mamaki-testnet.md#mamaki-testnet-faucet).

#### Optional: run the bridge node with a custom key

Afin d'exécuter un Bridge node en utilisant une clé personnalisée:

1. La clé personnalisée doit exister dans le répertoire du Bridge Node Celestia au bon chemin (default: `~/.celestia-bridge/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: start the bridge node with SystemD

Suivez le tutoriel sur la configuration du Bridge node en tant que processus de fond avec SystemD [ici](./systemd.md#celestia-bridge-node).

Vous avez configuré avec succès un Bridge node qui se synchronise avec le réseau.
