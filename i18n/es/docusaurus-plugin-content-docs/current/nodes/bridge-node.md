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

Desde una perspectiva de implementación, los Bridge Nodes ejecutan dos procesos separados:

1. Celestia App con Celestia Core ([ver repo](https://github.com/celestiaorg/celestia-app))

    * **Celestia App** es la máquina de estado donde se ejecuta la aplicación y la lógica de prueba de participación. Celestia App está construida en [Cosmos SDK](https://docs.cosmos.network/) y también engloba **Celestia Core**.
    * **Celestia Core** es la interacción con el estado, el consenso y la capa de producción de bloques. Celestia Core está construido sobre[nTendermint Core](https://docs.tendermint.com/), modificado para almacenar raíces de datos de bloques codificados de borrado entre otros cambios ([ver ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

2. Nodo Celestia ([ver repositorio](https://github.com/celestiaorg/celestia-node))

    * **Celestia Node** incrementa lo anterior con una red libp2p independiente que sirve solicitudes de muestreo de disponibilidad de datos. El equipo a veces se refiere a esto como la red "halo".

## Requisitos de hardware

Se recomiendan los siguientes requisitos mínimos de hardware para ejecutar el nodo bridge:

* Memoria: 8 GB RAM
* CPU: 4 cores
* Disco: 250 GB de almacenamiento SSD
* Ancho de banda: 1 Gbps descarga/100 Mbps subida

## Configurando tu nodo bridge

El siguiente tutorial se ha realizado sobre una versión de Ubuntu Linux 20.04 (LTS) x64.

### Actualizando las dependencias

Sigue las instrucciones para instalar las dependencias [aquí](../developers/environment.md).

## Despliega el nodo celestia

### Instala celestia-node

Instala el binario Celestia Node, que se utilizará para ejecutar el Bridge Node.

Sigue el tutorial sobre la instalación de Celestia App [aquí](../developers/celestia-node.md).

### Inicializa el nodo bridge

Ejecuta las siguientes instrucciones:

```sh
celestia bridge init --core.remote tcp://<ip-address>:26657
```

Si necesitas una lista de puertos RPC para conectarte, puedes comprobar la lista [aquí](./mamaki-testnet.md#rpc-endpoints)

### Ejecuta el nodo de puente

Inicia el Bridge Node con una conexión al endpoint gRPC de un nodo validador (que normalmente está en el puerto 9090):

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
