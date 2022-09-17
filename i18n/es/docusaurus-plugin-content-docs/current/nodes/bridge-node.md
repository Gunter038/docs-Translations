- - -
sidebar_label : Nodo Bridge
- - -

# Configurar un Nodo Celestia Bridge

Este tutorial recorrerá los pasos para configurar tu nodo de Celestia.

Los nodos Bridge conectan la capa de disponibilidad de datos y la capa de consenso al mismo tiempo que tienen la opción de convertirse en validador.

Los validadores no tienen que ejecutar nodos Bridge, pero se les anima a transmitir bloques a la red de disponibilidad de datos.

## Vista general de los nodos Bridge

Un nodo Bridge de Celestia tiene las siguientes propiedades:

1. Importar y procesar encabezados "raw" & bloques desde un proceso de Core confiable (lo que significa una conexión RPC de confianza con un nodo-core celestia) en la red de Consenso.  Los nodos Bridge pueden ejecutar este proceso Core internamente (incrusado) o simplemente conectarse a un punto final remoto. Los nodos Bridge también tienen la opción de ser un validador activo en la red del Consenso.
2. Validar y borrar el código de los bloques "raw".
3. Suministrar bloques compartidos con encabezados de disponibilidad de datos a Nodos Light en la red DA. ![bridge-node-diagram](/img/nodes/BridgeNodes.png)

From an implementation perspective, Bridge Nodes run two separate processes:

1. Celestia App with Celestia Core ([see repo](https://github.com/celestiaorg/celestia-app))

    * **Celestia App** is the state machine where the application and the proof-of-stake logic is run. Celestia App is built on [Cosmos SDK](https://docs.cosmos.network/) and also encompasses **Celestia Core**.
    * **Celestia Core** is the state interaction, consensus and block production layer. Celestia Core is built on [Tendermint Core](https://docs.tendermint.com/), modified to store data roots of erasure coded blocks among other changes ([see ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Celestia Node ([see repo](https://github.com/celestiaorg/celestia-node))

    * **Celestia Node** augments the above with a separate libp2p network that serves data availability sampling requests. The team sometimes refer to this as the “halo” network.

## Hardware requirements

The following hardware minimum requirements are recommended for running the bridge node:

* Memory: 8 GB RAM
* CPU: Quad-Core
* Disk: 250 GB SSD Storage
* Bandwidth: 1 Gbps for Download/100 Mbps for Upload

## Setting up your bridge node

The following tutorial is done on an Ubuntu Linux 20.04 (LTS) x64 instance machine.

### Setup the dependencies

Follow the tutorial here installing the dependencies [here](../developers/environment.md).

## Deploy the Celestia bridge node

### Install Celestia node

Install the Celestia Node binary, which will be used to run the Bridge Node.

Follow the tutorial for installing Celestia Node [here](../developers/celestia-node.md).

### Initialize the bridge node

Run the following:

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

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

#### Optional: run the bridge node with a custom key

In order to run a bridge node using a custom key:

1. The custom key must exist inside the celestia bridge node directory at the correct path (default: `~/.celestia-bridge/keys/keyring-test`)
2. The name of the custom key must be passed upon `start`, like so:

```sh
celestia bridge start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Optional: start the bridge node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.
