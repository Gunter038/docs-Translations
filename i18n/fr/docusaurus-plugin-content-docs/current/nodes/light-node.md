- - -
sidebar_label : Light Node
- - -

# Configuration d'un Light Node Celestia

Ce tutoriel vous guidera à travers la mise en place d'un Light Node Celestia, qui vous permettra d'effectuer des échantillonnages de disponibilité de données sur le réseau de disponibilité de données (Data Availability/DA).

> Pour voir un tutoriel vidéo sur la mise en place d'un Light Node Celestia, cliquez [ici](../developers/light-node-video.md)

## Présentation des Light Nodes

Les Light nodes s'assurent de la disponibilité des données. C'est le moyen le plus commun d'interagir avec le réseau Celestia.

![Light Node](/img/nodes/LightNodes.png)

Les Light Nodes ont le comportement suivant :

1. Ils écoutent les ExtendedHeaders, c'est-à-dire les en-têtes « bruts» enveloppés, qui notifient les nodes Celestia des nouveaux en-têtes de blocs et des métadonnées DA pertinentes.
2. Ils effectuent l'échantillonnage de la disponibilité des données (DAS) sur les en-têtes reçus.

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter un Light node :

* Mémoire: 2 Go de RAM
* CPU: Single Core
* Disque: 5 Go de stockage SSD
* Bande passante : 56 Gbps pour le téléchargement/56 Mbps pour l'upload

## Configuration de votre Light Node

Le tutoriel suivant est fait sur une machine d'instance Linux Ubuntu 20.04 (LTS) x64.

### Configurer les dépendances

Premièrement vérifiez que votre système d'exploitation est à jour ou mettez-le à jour :

```sh
# Si vous utilisez le gestionnaire de package APT
sudo apt update && sudo apt upgrade -y

# Si vous utilisez le gestionnaire de package YUM 
sudo yum update
```

Ce sont des paquets essentiels pour exécuter de nombreuses tâches comme le téléchargement de fichiers, la compilation et la surveillance du node:

```sh
# Si vous utilisez le gestionnaire de package APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# Si vous utilisez le gestionnaire de package YUM 
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### Installer Golang

Celestia-app et celestia-node sont codés en [Golang](https://go.dev/) donc nous devons installer Golang pour les construire et les exécuter.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Maintenant nous devons ajouter le répertoire `/usr/local/go/bin` à `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Pour vérifier que Go a été installé correctement, lancez :

```sh
go version
```

Le résultat devrait être la version installée suivante :

```sh
go version go1.18.2 linux/amd64
```

### Installer un nœud Celestia

Une chose à faire ici est de décider quelle version du nœud Celestia vous décidez de compiler. Le testnet Mamaki requiert v0.3.0-rc2 et le Devnet Arabica requiert v0.3.0.

Les sections suivantes soulignent comment l'installer pour les deux réseaux.

#### Installation du devnet Arabica

Installez le binaire du nœud Celestia en exécutant les commandes suivantes :

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

Vérifiez que le binaire fonctionne et la version avec la commande Celestia ci-dessous :

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

#### Installation du testnet Mamaki

Installez le binaire du nœud Celestia en exécutant les commandes suivantes :

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

Vérifiez que le binaire fonctionne et la version avec la commande Celestia ci-dessous :

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Initialiser le Light Node

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

Concernant les ports :

> NOTE : Le port `--core.grpc.port` est configuré par défaut à 9090, donc si vous n'en spécifiez pas un autre dans la ligne de commande, il s'exécutera sur celui-là par défaut. Vous pouvez utiliser le drapeau pour spécifier un autre port si vous le préférez.

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

#### Configuration d'Arabica

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./arabica-devnet.md#rpc-endpoints)

Par exemple, votre commande pourrait ressembler à cela :

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

#### Configuration de Mamaki

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

Par exemple, votre commande pourrait ressembler à cela :

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://rpc-mamaki.pops.one --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Clés et portefeuilles

Vous pouvez créer votre clé pour votre node en exécutant la commande suivante :

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

Une fois que vous démarrez le Light Node, une clé de wallet sera générée pour vous. Vous aurez besoin d'approvisionner cette adresse avec des jetons du testnet Mamaki pour payer les transactions de type PayForData.

Vous pouvez trouver cette adresse en exécutant la commande suivante dans le répertoire `celestia-node` :

```sh
./cel-key list --node.type light --keyring-backend test
```

Vous avez le choix entre deux réseaux pour obtenir des jetons de testnet :

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE : Si vous souhaitez lancer un light node pour votre rollup souverain, il est hautement recommandé de demander des jetons du devnet Arabica car c'est le devnet qui reçoit les dernières mises à jour utiles au développement de votre rollup souverain. Vous pouvez également utiliser le testnet Mamaki, bien qu'il soit davantage conçu pour les opérations de Validateurs.

Vous pouvez demander des fonds à l'adresse de votre portefeuille en utilisant la commande suivante sur Discord :

```console
$request <Wallet-Address>
```

Dans laquelle `<Wallet-Address>` est l'adresse `celestia1******` générée quand vous avez créé le wallet.

### Optionnel : exécuter le Light Node avec une clé personnalisée

Afin d'exécuter un Light Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Light Node Celestia au bon chemin (default: `~/.celestia-light/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <name_of_custom_key>
```
<!-- markdownlint-enable MD013 -->

### Optionnel : démarrer un light node avec SystemD

Suivez le tutoriel sur la configuration du Light Node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-light-node).

## Échantillonnage de disponibilité des données (DAS)

Avec votre light node en cours d'exécution, vous pouvez consulter ce tutoriel sur la soumission de transactions `PayForData` [ici](../developers/node-tutorial.md).
