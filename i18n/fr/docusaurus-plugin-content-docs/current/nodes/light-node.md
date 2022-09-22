- - -
sidebar_label : Light Node
- - -

# Setting up a Celestia Light Node

Ce tutoriel vous guidera à travers la mise en place d'un Light Node Celestia, qui vous permettra d'effectuer des échantillonnages de disponibilité de données sur le réseau de disponibilité de données (Data Availability/DA).

> Pour voir un tutoriel vidéo pour la mise en place d'un Light Node Celestia, cliquez [ici](../developers/light-node-video.md)

## Overview of light nodes

Les Light nodes s'assurent de la disponibilité des données. C'est le moyen le plus commun d'interagir avec le réseau Celestia.

![light-node](/img/nodes/LightNodes.png)

Les Light Nodes ont le comportement suivant :

1. Ils écoutent les ExtendedHeaders, c'est-à-dire les en-têtes « bruts» enveloppés, qui notifient les nodes Celestia des nouveaux en-têtes de blocs et des métadonnées DA pertinentes.
2. Ils effectuent l'échantillonnage de la disponibilité des données (DAS) sur les en-têtes reçus.

## Hardware requirements

Les exigences matérielles minimales suivantes sont recommandées pour exécuter un Light node :

* Mémoire: 2 Go de RAM
* CPU : Noyau unique
* Disque: 5 Go de stockage SSD
* Bande passante : 56 Gbps pour le téléchargement/56 Mbps pour l'upload

## Setting up your light node

Le tutoriel suivant est fait sur une machine d'instance Ubuntu Linux 20.04 (LTS) x64.

### Setup the dependencies

First, make sure to update and upgrade the OS:

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

These are essential packages that are necessary to execute many tasks like downloading files, compiling, and monitoring the node:

```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### Install Golang

Celestia-app and celestia-node are written in [Golang](https://go.dev/) so we must install Golang to build and run them.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Now we need to add the `/usr/local/go/bin` directory to `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

To check if Go was installed correctly run:

```sh
go version
```

The output should be the version installed:

```sh
go version go1.18.2 linux/amd64
```

### Install Celestia node

One thing to note here is deciding which version of celestia-node you wish to compile. Mamaki Testnet requires v0.3.0-rc2 and Arabica Devnet requires v0.3.0.

The following sections highlight how to install it for the two networks.

#### Mamaki Testnet installation

Install the celestia-node binary by running the following commands:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

#### Arabica Devnet installation

Install the celestia-node binary by running the following commands:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

## Initialize the light node

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

### Start the light node

Démarre le Light node avec une connexion au point de terminaison gRPC d'un node de validateur (qui est généralement exposé sur le port 9090):

> REMARQUE : Pour avoir accès à la possibilité d'obtenir/de soumettre des informations relatives à l'état, telles que la possibilité de soumettre des transactions PayForData ou d'interroger solde de compte du node, un point de terminaison gRPC d'un Node Validateur (core) doit être transmis comme montré ci-dessous.

For ports:

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./arabica-devnet.md#rpc-endpoints)

For example, your command might look something like this:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Keys and wallets

You can create your key for your node by running the following command:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

Une fois que vous démarrez le Light Node, une clé de wallet sera générée pour vous. You will need to fund that address with testnet tokens to pay for PayForData transactions.

You can find the address by running the following command in the `celestia-node` directory:

```sh
./cel-key list --node.type light --keyring-backend test
```

You have two networks to get testnet tokens from:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTE: If you are running a light node for your sovereign rollup, it is highly recommended to request Arabica devnet tokens as Arabica has the latest changes that can be used to test for developing your sovereign rollup. You can still use Mamaki Testnet as well, it is just used for Validator operations.

You can request funds to your wallet address using the following command in Discord:

```console
$request <Wallet-Address>
```

Where `<Wallet-Address>` is the `celestia1******` address generated when you created the wallet.

### Optional: run the light node with a custom key

Afin d'exécuter un Light Node en utilisant une clé personnalisée :

1. La clé personnalisée doit exister dans le répertoire du Light Node Celestia au bon chemin (default: `~/.celestia-light/keys/keyring-test`)
2. Le nom de la clé personnalisée doit être transmis lors de `start`, comme ceci:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <name_of_custom_key>
```
<!-- markdownlint-enable MD013 -->

### Optional: start light node with SystemD

Suivez le tutoriel sur la configuration du Light Node en tant que processus d'arrière-plan avec SystemD [ici](./systemd.md#celestia-light-node).

## Data availability sampling (DAS)

Avec votre light node en cours d'exécution, vous pouvez consulter ce tutoriel sur la soumission de transactions `PayForData` [ici](../developers/node-tutorial.md).
