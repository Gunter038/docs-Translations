- - -
sidebar_label : Nodo Bridge
- - -

# Configurar un Nodo Celestia Bridge

Este tutorial recorrerá los pasos para configurar tu nodo de Celestia.

Los nodos Bridge conectan la capa de disponibilidad de datos y la capa de consenso al mismo tiempo que tienen la opción de convertirse en validador.

Los validadores no tienen que ejecutar nodos bridge, pero se les anima a transmitir bloques a la red de disponibilidad de datos.

## Vista general de los nodos Bridge

Un nodo Bridge de Celestia tiene las siguientes propiedades:

1. Importar y procesar encabezados "raw" & bloques desde un proceso de Core confiable (lo que significa una conexión RPC de confianza con un nodo-core celestia) en la red de Consenso.  Los nodos Bridge pueden ejecutar este proceso Core internamente (incrusado) o simplemente conectarse a un punto final remoto. Los nodos Bridge también tienen la opción de ser un validador activo en la red del Consenso.
2. Validar y borrar el código de los bloques "raw".
3. Suministrar bloques compartidos con encabezados de disponibilidad de datos a Nodos Light en la red DA. ![diagrama de nodo-bridge](/img/nodes/BridgeNodes.png)

Desde una perspectiva de implementación, los Bridge Nodes ejecutan dos procesos separados:

1. Celestia App con Celestia Core ([ver repo](https://github.com/celestiaorg/celestia-app))

    * **Celestia App** es la máquina de estado donde se ejecuta la aplicación y la lógica de prueba de participación. Celestia App está construida en [Cosmos SDK](https://docs.cosmos.network/) y también engloba **Celestia Core**.
    * **Celestia Core** es la interacción del estado, el consenso y la capa de producción de bloques. Celestia Core está construida sobre[Tendermint Core](https://docs.tendermint.com/), modificada para almacenar raíces de datos de bloques codificados de borrado entre otros cambios ([ver ADRs](https://github.com/celestiaorg/celestia-core/tree/master/docs/celestia-architecture)).

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
celestia bridge init --core.ip <ip-address> --core.rpc.port <port>
```

> NOTA: El `--core.rpc.port` por defecto es 26657, así que si no lo especificas en la línea de comandos, se establecerá por defecto en ese puerto. Puedes utilizar la bandera para especificar otro puerto si lo prefieres.

Si necesitas una lista de puertos RPC para conectarte, puedes comprobar la lista [aquí](./mamaki-testnet.md#rpc-endpoints)

### Ejecuta el nodo bridge

Inicia el Bridge Node con una conexión al endpoint gRPC de un nodo validador (que normalmente está en el puerto 9090):

```sh
celestia bridge start --core.ip <ip-address> --core.grpc.port <port>
```

> NOTA: El `--core.grpc.port` es por defecto 9090, así que si no lo especificas en la línea de comandos, se establecerá por defecto en ese puerto. Puedes utilizar la bandera para especificar otro puerto si lo prefieres.

Si necesitas una lista de puertos RPC para conectarte, puedes comprobar la lista [aquí](./mamaki-testnet.md#rpc-endpoints)

Puedes crear tu clave para tu nodo siguiendo las instrucciones de `cel-key` [aquí](./keys.md)

Una vez que inicies el Bridge Node, se generará una clave de wallet para ti. Tendrás que enviar a esa dirección los tokens de Mamaki Testnet para pagar por transacciones de PayForData. Puedes encontrar la dirección ejecutando el siguiente comando:

```sh
./cel-key list --node.type bridge --keyring-backend test
```

Tienes dos redes desde las que obtener tokens de testnet:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> NOTA: Si estás ejecutando un nodo bridge para tu validador es altamente recomendable solicitar tokens de Mamaki testnet ya que esta es la testnet utilizada para probar las operaciones de validador.

#### Opcional: ejecutar el nodo bridge con una key personalizada

Para ejecutar un nodo bridge usando una clave personalizada:

1. La clave personalizada debe existir dentro del directorio de nodos celestia en la ruta correcta (por defecto: `~/.celestia-bridge/keys/keyring-test`)
2. El nombre de la clave personalizada debe pasarse al `start`, así:

```sh
celestia bridge start --core.ip <ip> --core.grpc.port 9090 --keyring.accname <name_of_custom_key>
```

### Opcional: iniciar el nodo bridge con SystemD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#celestia-bridge-node).

Has configurado con éxito un nodo bridge que está sincronizando con la red.
