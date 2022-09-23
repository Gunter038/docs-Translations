- - -
sidebar_label : Light Node
- - -

# Configurando un Celestia Light Node

Este tutorial te guiará a través de la configuración de un nodo light Celestia, que te permitirá realizar muestreo de disponibilidad de datos en la red de datos de disponibilidad (DA).

> Para ver un video tutorial para configurar un nodo Celestia, haz clic [aquí](../developers/light-node-video.md)

## Resumen de los light nodes

Los light nodes garantizan la disponibilidad de datos. Esta es la forma más común de interactuar con la red Celestia.

![light nodes (Nodos ligeros)](/img/nodes/LightNodes.png)

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
ver="1.19.1"
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

One thing to note here is deciding which version of celestia-node you wish to compile. Mamaki Testnet requires v0.3.0-rc2 and Arabica Devnet requires v0.3.0.

The following sections highlight how to install it for the two networks.

#### Mamaki Testnet installation

Instala el binario celestia-node ejecutando los siguientes comandos:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

Verifica que el binario está funcionando y comprueba la versión con el comando de versión celestia:

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

For ports:

> NOTE: The `--core.grpc.port` defaults to 9090, so if you do not specify it in the command line, it will default to that port. You can use the flag to specify another port if you prefer.

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./arabica-devnet.md#rpc-endpoints)

Por ejemplo, tu comando podría verse algo como esto:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Claves y wallets

Puedes crear tu clave para tu nodo ejecutando el siguiente comando:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

Una vez que inicies el Light Node, se generará una clave de wallet para ti. You will need to fund that address with testnet tokens to pay for PayForData transactions.

Puedes encontrar la dirección ejecutando el siguiente comando en el directorio `celestia-node`:

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

### Opcional: ejecutar el light node con una tecla personalizada

Para ejecutar un light node usando una clave personalizada:

1. La clave personalizada debe existir dentro del directorio de nodos celestia en la ruta correcta (por defecto: `~/.celestia-bridge/keys/keyring-test`)
2. El nombre de la clave personalizada debe pasarse al `start`, así:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <name_of_custom_key>
```
<!-- markdownlint-enable MD013 -->

### Opcional: iniciar light node con SystememD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#celestia-light-node).

## Muestreo de disponibilidad de datos (DAS)

Con tu light node en ejecución, puedes revisar este tutorial de enviar `transacciones de PayForData` [aquí](../developers/node-tutorial.md).
