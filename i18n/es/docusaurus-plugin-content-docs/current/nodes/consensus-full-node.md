- - -
sidebar_label : Consenso de Full Node
- - -

# Configurar un Celestia Full Node de Consenso
<!-- markdownlint-disable MD013 -->

Los Full Nodes de consenso te permiten sincronizar el historial de la blockchain en la capa de Consenso de Celestia.

## Requisitos de hardware

Se recomiendan los siguientes requisitos mínimos de hardware para ejecutar el nodo bridge:

* Memoria: 8 GB RAM
* CPU: 4 cores
* Disco: 250 GB de almacenamiento SSD
* Ancho de banda: 1 Gbps descarga/100 Mbps subida

## Configurando tu full node de consenso

El siguiente tutorial se ha realizado sobre una versión de Ubuntu Linux 20.04 (LTS) x64.

### Actualizando las dependencias

Sigue las instrucciones para instalar las dependencias [aquí](../developers/environment.md).

## Desplegando celestia-app

Esta sección describe la parte 1 de la configuración del full node de Celestia: ejecutando un daemon de la aplicación Celestia con un nodo interno Core de Celestia.

> Nota: Asegúrate de tener al menos 100+ Gb de espacio libre para instalar y ejecutar de forma segura el full node.

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

### Restablecer la red

Esto eliminará todas las carpetas de datos para que podamos comenzar desde el inicio:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Opcional: sincronización rápida con snapshot

Sincronizar desde Génesis puede tomar mucho tiempo, dependiendo de tu hardware. Utilizando este método puedes sincronizar tu nodo Celestia muy rápidamente descargando un snapshot reciente de la blockchain. Si prefieres sincronizar desde el Génesis, entonces puedes saltarte esta parte.

Si deseas usar snapshot, elige la red a la que deseas sincronizar de la lista de abajo:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Iniciar celestia-app

Para iniciar el full node del consenso, ejecuta lo siguiente:

```sh
celestia-appd start
```

Esto te permitirá sincronizar el historial de la blockchain de Celestia.

### Opcional: configurar para RPC endpoint

Puedes configurar tu Full Node de consenso para que sea un endpoint RPC público y escuchar cualquier conexión desde los Data Availability Nodes para servir solicitudes para la API de disponibilidad de datos [aquí](../developers/node-tutorial.md).

Ten en cuenta que necesitarás asegurar que el puerto 9090 esté abierto para esto.

Ejecuta los siguientes comandos:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

Reinicia `celestia-appd` en el paso anterior para cargar esas configuraciones.

### Iniciar celestia-app con SystemD

Sigue el tutorial sobre cómo configurar Celestia-App como un proceso en segundo plano con SystemD [aquí](./systemd.md#start-the-celestia-app-with-systemd).
