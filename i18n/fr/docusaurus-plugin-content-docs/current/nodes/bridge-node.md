- - -
sidebar_label : Bridge Node
- - -

# Configurer un Bridge Node Celestia

Ce tutoriel passera en revue les étapes de configuration de votre Bridge node Celestia (Noeud Pont).

Les Bridge nodes connectent la couche de disponibilité des données et la couche de consensus tout en offrant la possibilité de devenir validateur.

## Présentation des Bridge Nodes

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

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le Bridge node :

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configuration de votre Bridge node

The following tutorial is done on an Ubuntu Linux 20.04 (LTS) x64 instance machine.

### Setup The Dependencies

Follow the tutorial here installing the dependencies [here](../developers/environment.md).

## Deploy the Celestia Bridge Node

### Install Celestia Node

Install the Celestia Node binary, which will be used to run the Bridge Node.

Follow the tutorial for installing Celestia Node [here](../developers/celestia-node.md).

### Initialize the Bridge Node

Run the following:

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657 
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

### Run the Bridge Node

Start the Bridge Node with a connection to a validator node's gRPC endpoint (which is usually exposed on port 9090):

> NOTE: In order for access to the ability to get/submit state-related information, such as the ability to submit PayForData transactions, or query for the node's account balance, a gRPC endpoint of a validator (core) node must be passed as directed below._

```sh
celestia bridge start --core.grpc http://<ip>:9090
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

You can create your key for your node by following the `cel-key` instructions [here](./keys.md)

Once you start the Bridge Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions. You can find the address by running the following command:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Mamaki Testnet tokens can be requested [here](./mamaki-testnet.md#mamaki-testnet-faucet).

#### Optional: Run the Bridge Node with a Custom Key

In order to run a bridge node using a custom key:

1. The custom key must exist inside the celestia bridge node directory at the correct path (default: `~/.celestia-bridge/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: Start the Bridge Node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.
