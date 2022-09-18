- - -
sidebar_label : Full Storage Node
- - -

# Configurar un Full Storage Node en Celestia

Este tutorial te guiará a través de la configuración de un nodo Full Storage en Celestia, que es un nodo Celestia que no se conecta a Celestia App (por lo tanto, no es un full node) pero almacena todos los datos.

## Requisitos de hardware

Se recomiendan los siguientes requisitos mínimos de hardware para ejecutar el full storage node:

* Memoria: 8 GB RAM
* CPU: 4 cores
* Disco: 250 GB de almacenamiento SSD
* Ancho de banda: 1 Gbps descarga/100 Mbps subida

## Configurando tu nodo validador

El siguiente tutorial se ha realizado sobre una versión de Ubuntu Linux 20.04 (LTS) x64.

### Actualizando las dependencias

Puedes seguir el tutorial para configurar tus dependencias [aquí](../developers/environment.md)

## Instalar el nodo Celestia

> Nota: Asegúrate de tener al menos 250 Gb de espacio libre para el Celestia Full Storage Node

Puedes seguir el tutorial para instalar Celestia Node [aquí](../developers/celestia-node.md)

### Ejecutar el ull storage node

#### Inicializar el full storage node

Ejecuta el siguiente comando:

```sh
celestia full init
```

#### Inicializar el full storage node

Inicia el Full Storage Node con una conexión al endpoint gRPC de un nodo validador (que normalmente está en el puerto 9090):

> NOTA: Para acceder a la capacidad de obtener/enviar información relacionada con el estado, como la posibilidad de enviar transacciones de PayForData o consulta para el saldo de cuenta del nodo un endpoint gRPC de un nodo validador (core) debe ser configurado como indica debajo.

```sh
celestia full start --core.grpc http://<ip addr of core node>:9090
```

Si desea encontrar los endpoints RPC de ejemplo, consulte la lista de recursos [aquí](./mamaki-testnet.md#rpc-endpoints).

Puedes crear tu clave para tu nodo siguiendo las instrucciones de `cel-key-` [aquí](./keys.md)

Una vez que inicies el Full Node, se generará una clave de wallet para ti. Tendrás que enviar a esa dirección los tokens de Mamaki Testnet para pagar por transacciones de PayForData. Puedes encontrar la dirección ejecutando el siguiente comando:

```sh
./cel-key list --node.type full --keyring-backend test
```

Los tokens de Mamaki Testnet pueden ser solicitados [aquí](./mamaki-testnet.md#mamaki-testnet-faucet).

### Opcional: ejecuta el full storage node con una clave personalizada

Para ejecutar un full storage usando una clave personalizada:

1. La clave personalizada debe existir dentro del directorio de nodos celestia en la ruta correcta (por defecto: `~/.celestia-bridge/keys/keyring-test`)
2. El nombre de la clave personalizada debe pasarse al `start`, así:

```sh
celestia full start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Opcional: ejecuta el full storage node con SystemD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#celestia-full-storage-node).

Con eso, ahora se está ejecutando un nodo Celestia Full Storage.
