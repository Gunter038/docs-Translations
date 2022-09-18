- - -
sidebar_label : Light Node
- - -

# Configurando un Celestia Light Node

Este tutorial te guiará a través de la configuración de un nodo light Celestia, que te permitirá realizar muestreo de disponibilidad de datos en la red de datos de disponibilidad (DA).

> Para ver un video tutorial para configurar un nodo Celestia, haz clic [aquí](../developers/light-node-video.md)

## Resumen de los light nodes

Los light nodes garantizan la disponibilidad de datos. Esta es la forma más común de interactuar con la red Celestia.

![light-node](/img/nodes/LightNodes.png)

Los light nodes tienen el siguiente comportamiento:

1. Escuchan ExtendedHeaders, es decir, cabeceras envueltas "raw", que notifican a los nodos Celestia acerca de nuevos encabezados de bloque y metadatos relevantes de DA.
2. Realizan muestreo de disponibilidad de datos (DAS) en los encabezados recibidos

## Requisitos de hardware

Se recomiendan los siguientes requisitos mínimos de hardware para ejecutar un light node:

* Memoria: 2 GB RAM
* CPU: Núcleo único
* Disco: 5 GB de almacenamiento SSD
* Ancho de banda: 56 Gbps descarga/56 Mbps subida

## Configurando tu light node

El siguiente tutorial se ha realizado sobre una versión de Ubuntu Linux 20.04 (LTS) x64.

### Actualizando las dependencias

Primero, asegúrate de actualizar el sistema operativo:

```sh
# Si estás usando el gestor de paquetes APT
sudo apt update && sudo apt upgrade -y

# Si estás usando el gestor de paquetes YUM
sudo yum update
```

Estos son paquetes esenciales que son necesarios para ejecutar muchas tareas como descargar archivos, compilar y monitorear el nodo:

```sh
# Si está usando el gestor de paquetes APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# Si está usando el gestor de paquetes YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y 
```

### Instalando Golang

Celestia-app y celestia-node están escritos en [Golang](https://go.dev/) por lo que debemos instalar Golang para compilarlos y ejecutarlos.

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Ahora necesitamos añadir el directorio `/usr/local/go/bin` a `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Para comprobar si Go fue instalado correctamente:

```sh
go version
```

El resultado debe ser la versión instalada:

```sh
go version go1.18.2 linux/amd64
```

### Instalar el nodo Celestia

Instala el binario celestia-node ejecutando los siguientes comandos:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Verifica que el binario está funcionando y comprueba la versión con el comando de versión celestia:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Inicializa el light node

Ejecuta el siguiente comando:

```sh
celestia light init
```

Deberás ver algo como esto:

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### Iniciar el light node

Inicia el Bridge Node con una conexión al endpoint gRPC de un nodo validador (que normalmente está en el puerto 9090):

> NOTA: Para acceder a la capacidad de obtener/enviar información relacionada con el estado, como la posibilidad de enviar transacciones de PayForData o consulta para el saldo de cuenta del nodo un endpoint gRPC de un nodo validador (core) debe ser configurado como indica debajo.

```sh
celestia light start --core.grpc http://<ip>:9090
```

Si necesitas una lista de puertos RPC para conectarte, puedes comprobar la lista [aquí](./mamaki-testnet.md#rpc-endpoints)

Por ejemplo, tu comando podría verse algo como esto:

```sh
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090
```

### Claves y wallets

Puedes crear tu clave para tu nodo ejecutando el siguiente comando:

```sh
make cel-key
```

Una vez que inicies el Light Node, se generará una clave de wallet para ti. Tendrás que enviar a esa dirección los tokens de Mamaki Testnet para pagar por transacciones de PayForData.

Puedes encontrar la dirección ejecutando el siguiente comando en el directorio `celestia-node`:

```sh
./cel-key list --node.type light --keyring-backend test
```

Los tokens de Mamaki Testnet pueden ser solicitados [aquí](./mamaki-testnet.md#mamaki-testnet-faucet).

### Opcional: ejecutar el light node con una tecla personalizada

Para ejecutar un light node usando una clave personalizada:

1. La clave personalizada debe existir dentro del directorio de nodos celestia en la ruta correcta (por defecto: `~/.celestia-bridge/keys/keyring-test`)
2. El nombre de la clave personalizada debe pasarse al `start`, así:

```sh
celestia light start --core.grpc http://<ip>:9090 --keyring.accname <name_of_custom_key>
```

### Opcional: iniciar light node con SystememD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#celestia-light-node).

## Muestreo de disponibilidad de datos (DAS)

With your light node running, you can check out this tutorial on submitting `PayForData` transactions [here](../developers/node-tutorial.md).
