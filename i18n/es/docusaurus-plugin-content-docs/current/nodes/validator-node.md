- - -
sidebar_label : nodo validador
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

> Nota: Asegúrate de tener al menos 100+ Gb de espacio libre para instalar y ejecutar de forma segura el nodo validador.

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

A continuación, selecciona la red que desea utilizar para delegar a un validador:

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Despliega el nodo celestia

Esta sección describe la parte 2 de la configuración del Nodo Validador de Celestia: ejecutar un daemon del Nodo Bridge Celestia.

### Instala celestia-node

Puedes seguir el tutorial para instalar Celestia Node [aquí](../developers/celestia-node.md)

### Inicializar el nodo de bridge

Ejecuta las siguientes instrucciones:

```sh
celestia bridge init --core.ip <ip-address> --core.grpc.port <port>
```

> NOTA: El `--core.grpc. ort` es por defecto 9090, así que si no lo especificas en la línea de comandos, se establecerá por defecto en ese puerto. Puedes utilizar la bandera para especificar otro puerto si lo prefieres.

Si necesitas una lista de puertos RPC para conectarte, puedes comprobar la lista [aquí](./mamaki-testnet.md#rpc-endpoints)

### Ejecutar el nodo bridge

Ejecuta las siguientes instrucciones:

```sh
celestia bridge start
```

### Opcional: iniciar el nodo bridge con SystemD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#celestia-bridge-node).

Has configurado con éxito un nodo bridge que está sincronizando con la red.

## Ejecutar un nodo validador

Después de completar todos los pasos necesarios, ¡ya estás listo para ejecutar un validador! Para crear tu validador on-chain, sigue las instrucciones que se indican a continuación. Ten en cuenta que estos pasos son necesarios SOLO si quieres participar en el consenso.

¡Elige un nombre de `moniker` a tu elección! Este es el nombre del validador que se mostrará en los paneles públicos y exploradores. `VALIDATOR_WALLET` debe ser el mismo que definiste anteriormente. El parámetro `--min-self-delegation=1000000` define la cantidad de tokens que se delegan de tu wallet de validador.

Ahora, conéctate a la red de tu elección.

Tienes la siguiente opción de conectar a la lista de redes mostrada a continuación:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Completa las instrucciones en la red respectiva que quieres validar en para completar el proceso de configuración del validador.
