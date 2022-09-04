- - -
sidebar_label : Validator Node
- - -

# Configurar un nodo Validador en Celestia

Los nodos validadores te permiten participar en el consenso de la red Celestia.

## Requisitos de hardware

Se recomiendan los siguientes requisitos mínimos de hardware para ejecutar el nodo validador:

* Memoria: 8 GB RAM
* CPU: 4 cores
* Disco: 250 GB de almacenamiento SSD
* Ancho de banda: 1 Gbps descarga/100 Mbps subida

## Configurando tu nodo validador

El siguiente tutorial se ha realizado sobre una versión de Ubuntu Linux 20.04 (LTS) x64.

### Actualizando las dependencias

Sigue las instrucciones para instalar las dependencias [aquí](../developers/environment.md).

## Desplegando celestia-app

Esta sección describe la parte 1 de la configuración del nodo validador de Celestia: ejecutando un daemon de la aplicación Celestia con un nodo interno Core de Celestia.

> Note: Make sure you have at least 100+ Gb of free space to safely install+run the Validator Node.

### Instalar celestia-app

Sigue el tutorial sobre la instalación de Celestia App [aquí](../developers/celestia-app.md).

### Configura las redes P2P

Para esta sección de la guía, selecciona la red a la que quieres conectar:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Después de esto, puedes continuar con el resto del tutorial.

### Configura pruning

Para un menor uso de espacio en disco, recomendamos configurar pruning utilizando las configuraciones que se muestran a continuación. Puedes cambiar esto a tus propias configuraciones de pruning si quieres:

```sh
PRUNING="custom"
PRUNING_KEEP_RECENT="100"
PRUNING_INTERVAL="10"

sed -i -e "s/^pruning *=.*/pruning = \"$PRUNING\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \
\"$PRUNING_KEEP_RECENT\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \
\"$PRUNING_INTERVAL\"/" $HOME/.celestia-app/config/app.toml
```

### Configurar el modo validador

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### Restablecer la red

Esto eliminará todas las carpetas de datos para que podamos comenzar desde el inicio:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Opcional: sincronización rápida con snapshot

Sincronizar desde Génesis puede tomar mucho tiempo, dependiendo de tu hardware. Utilizando este método puedes sincronizar tu nodo Celestia muy rápidamente descargando una instantánea reciente de la blockchain. Si quieres sincronizar desde el Génesis, entonces puedes saltarte esta parte.

Si deseas usar snapshot, eligela red a la que deseas sincronizar de la lista de abajo:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Iniciar celestia-app con SystemD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#start-the-celestia-app-with-systemd).

### Wallet

Siga el tutorial para crear una wallet [aquí](../developers/wallet.md).

### Delegar stake a un validador

Crear una variable de entorno para la dirección:

```sh
VALIDATOR_WALLET=<validator-address>
```

Si quieres delegar más stake a cualquier validador, incluyendo el tuyo propio necesitarás la dirección `celesvaloper` del validador en cuestión. Puedes comprobarlo usando el explorador de bloques mencionado anteriormente o puedes ejecutar el comando de abajo para obtener el `celesvaloper` de tu wallet validador local en caso de que quieras delegar más en él:

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

Después de introducir la contraseña de la wallet deberías ver una salida similar:

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

Next, select the network you want to use to delegate to a validator:

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Deploy the celestia-node

This section describes part 2 of Celestia Validator Node setup: running a Celestia Bridge Node daemon.

### Install celestia-node

You can follow the tutorial for installing Celestia Node [here](../developers/celestia-node.md)

### Initialize the bridge node

Run the following:

```sh
celestia bridge init --core.remote tcp://<ip:port of celestia-app> \
  --core.grpc http://<ip:port>
```

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

Run the following:

```sh
celestia bridge start
```

### Optional: start the bridge node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.

## Run a validator node

After completing all the necessary steps, you are now ready to run a validator! In order to create your validator on-chain, follow the instructions below. Keep in mind that these steps are necessary ONLY if you want to participate in the consensus.

Pick a `moniker` name of your choice! Pick a `moniker` name of your choice! This is the validator name that will show up on public dashboards and explorers. `VALIDATOR_WALLET` must be the same you defined previously. Parameter `--min-self-delegation=1000000` defines the amount of tokens that are self delegated from your validator wallet.

Now, connect to the network of your choice.

You have the following option of connecting to list of networks shown below:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Complete the instructions in the respective network you want to validate in to complete the validator setup process.
