- - -
sidebar_label : Bridge Node
- - -

# Configurer un Bridge Node Celestia

Ce tutoriel passera en revue les étapes de configuration de votre Bridge node Celestia (Noeud Pont).

Les Bridge nodes connectent la couche de disponibilité des données et la couche de consensus tout en offrant la possibilité de devenir validateur.

Les Validateurs n'ont pas à exécuter les nœuds de passerelle (bridge nodes) mais sont encouragés à relayer les blocs au réseau de disponibilité des données.

## Présentation des nœuds de passerelle (Bridge Nodes)

Un Bridge node Celestia a les propriétés suivantes:

1. Importez et traitez les en-têtes et blocs "bruts" à partir d'un processus Core de confiance (c'est-à-dire une connexion RPC de confiance à un node celestia-core) dans le Réseau consensuel. Les bridge nodes peuvent exécuter ce processus principal en interne (intégré) ou se connecter simplement à un terminal distant. Les bridge nodes offrent aussi la possibilité d'être un validateur actif dans le consensus du réseau .
2. Valider et effacer le code des blocs « bruts »
3. Fournir des parts de blocs avec des en-têtes de disponibilité des données aux Light Nodes du réseau DA (Data availabilty, disponibilité des données). ![bridge-node-diagram](/img/nodes/BridgeNodes.png)

Du point de vue de l'implémentation, les Bridge nodes exécutent deux processus distincts:

1. Celestia App avec Celestia Core ([voir le repo](https://github.com/celestiaorg/celestia-app))

    * **Celestia App** est la machine où l'application et la logique de Proof-of-Stake (Preuve d'enjeu) sont exécutées. Celestia App est construite sur le [Cosmos SDK](https://docs.cosmos.network/) et englobe également **Celestia Core**.
    * **Celestia Core** est l'interaction d'état, le consensus et la couche de production de blocs. Celestia Core est construit sur [Tendermint Core](https://docs.tendermint.com/), modifié pour stocker des racines de données "d'erasure block" parmi d'autres changements ([voir ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia Node ([voir le repo](https://github.com/celestiaorg/celestia-node))

    * **Celestia Node** augmente ce qui précède avec un réseau libp2p séparé qui sert des demandes d'échantillonnage de la disponibilité des données. L'équipe appelle parfois ceci le réseau « halo ».

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le Bridge node :

* Mémoire: 8 Go de RAM
* CPU: Quad-Core
* Disque: 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configuration de votre Bridge Node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Configurer les dépendances

Suivez le tutoriel ici pour installer les dépendances [ici](../developers/environment.md).

## Déployez le bridge node Celestia

### Installer Celestia Node

Installez le binaire Celestia Node, qui sera utilisé pour exécuter le Bridge node.

Suivez le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md).

### Initialiser le bridge node

Exécutez les commandes suivantes:

```sh
celestia bridge init --core.ip <ip-address> --core.rpc.port <port>
```

> Note : le port `--core.rpc.port` est configuré par défaut à 26657, donc si vous n'en spécifiez pas un autre dans la ligne de commande, il s'exécutera sur celui-là par défaut. Vous pouvez utiliser le drapeau pour spécifier un autre port si vous le préférez.

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

### Exécuter le Bridge Node

Démarrer le Bridge Node avec une connexion au terminal Rpc d'un node Validateur (qui est d'habitude exposé sur le port 9090):

```sh
celestia bridge start --core.ip <ip-address> --core.grpc.port <port>
```

> Note : le port `--core.grpc.port`  est configuré par défaut à 9090, donc si vous n'en spécifiez pas un autre dans la ligne de commande, il s'exécutera sur celui-là par défaut. Vous pouvez utiliser le drapeau pour spécifier un autre port si vous le préférez.

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

Vous pouvez créer votre clé pour votre node en suivant les instructions `cel-key` [ici](./keys.md)

Une fois que vous aurez démarré le Bridge Node, une clé de wallet sera générée pour vous. Vous aurez besoin d'approvisionner cette adresse avec des jetons du testnet pour payer les transactions de type PayForData. Vous pouvez trouver l'adresse en exécutant la commande suivante:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Vous avez deux réseaux pour obtenir des jetons de testnet :

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE : si vous exécutez un Bridge Node pour votre validateur il est hautement recommandé de demander des jetons du testnet Mamaki, car c'est le réseau le plus adapté aux opérations relatives aux Validateurs.

#### Optionnel : exécutez le Bridge Node avec une clé personnalisée

Afin d'exécuter un Bridge node en utilisant une clé personnalisée:

1. La clé personnalisée doit exister dans le répertoire du Bridge Node Celestia au bon chemin (default: `~/.celestia-bridge/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

```sh
celestia bridge start --core.ip <ip> --core.grpc.port 9090 --keyring.accname <name_of_custom_key>
```

### Optionnel : démarrer le Bridge Node avec SystemD

Suivez le tutoriel sur la configuration du Bridge node en tant que processus de fond avec SystemD [ici](./systemd.md#celestia-bridge-node).

Vous avez configuré avec succès un Bridge node qui se synchronise avec le réseau.
