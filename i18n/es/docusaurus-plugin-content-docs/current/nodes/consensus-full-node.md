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

This section describes part 1 of Celestia consensus full node setup: running a Celestia App daemon with an internal Celestia Core node.

> Note: Make sure you have at least 100+ Gb of free space to safely install + run the consensus full node.

### Install celestia-app

Follow the tutorial on installing Celestia App [here](../developers/celestia-app.md).

### Setup the P2P networks

For this section of the guide, select the network you want to connect to:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

After that, you can proceed with the rest of the tutorial.

### Configure pruning

For lower disk space usage we recommend setting up pruning using the configurations below. You can change this to your own pruning configurations if you want:

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

### Reset network

This will delete all data folders so we can start fresh:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Optional: quick-sync with snapshot

Syncing from Genesis can take a long time, depending on your hardware. Using this method you can synchronize your Celestia node very quickly by downloading a recent snapshot of the blockchain. If you would like to sync from the Genesis, then you can skip this part.

If you want to use snapshot, determine the network you would like to sync to from the list below:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Start the celestia-app

In order to start your consensus full node, run the following:

```sh
celestia-appd start
```

This will let you sync the Celestia blockchain history.

### Optional: configure for RPC endpoint

You can configure your Consensus Full Node to be a public RPC endpoint and listen to any connections from Data Availability Nodes in order to serve requests for the Data Availability API [here](../developers/node-tutorial.md).

Note that you would need to ensure port 9090 is open for this.

Run the following commands:

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0.0:26657"#g' ~/.celestia-app/config/config.toml
```

Restart `celestia-appd` in the previous step to load those configs.

### Start the celestia-app with SystemD

Follow the tutorial on setting up Celestia-App as a background process with SystemD [here](./systemd.md#start-the-celestia-app-with-systemd).
