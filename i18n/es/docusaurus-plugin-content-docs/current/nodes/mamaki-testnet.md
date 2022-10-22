---
sidebar_label: Mamaki Testnet
---

# Mamaki Testnet

![mamaki-testnet](/img/mamaki.png)

Esta guía contiene las secciones relevantes sobre cómo conectarse a Mamaki, dependiendo del tipo de nodo que esté ejecutando. Mamaki Testnet está diseñado para ayudar a los validadores a probar su infraestructura y software de nodos con la testnet. Se anima a los desarrolladores a desplegar sus rollups soberanos en Mamaki, pero también recomendamos [Arabica Devnet](./arabica-devnet.md) para eso ya que está diseñado para fines de desarrollo.

Mamaki es un hito en Celestia, permitiendo a todos probar las funcionalidades principales de la red. Lee el anuncio [aquí](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/).

Tu mejor enfoque para participar es determinar primero qué nodo deseas ejecutar. Cada guía de nodo se enlazará a la red relevante para mostrarte cómo conectarse a ellos.

Tienes una lista de opciones sobre el tipo de nodos que puedes ejecutar para participar en Mamaki:

Consenso:

* [Nodo Validador](./validator-node.md)
* [Full Node](./consensus-full-node.md)

Nodos de disponibilidad de datos:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Selecciona el tipo de nodo que deseas ejecutar y sigue las instrucciones en cada página correspondiente. Cada vez que se te pida que selecciones el tipo de red a la que desea conectarse en esas guías, selecciona `Mamaki` para referir a las instrucciones correctas en esta página sobre cómo conectarse a Mamaki.

## RPC endpoints

Hay una lista de RPC endpoints que puedes utilizar para conectarte a Mamaki Testnet:

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://grpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)
* [https://rpc.mamaki.celestia.counterpoint.software](https://rpc.mamaki.celestia.counterpoint.software)

## Mamaki Testnet faucet

> USAR ESTE FAUCER NO TE DA ACCESO A NINGÚN AIRDROP U OTRA DISTRIBUCIÓN DE TOKENS DE LA MAINNET DE CELESTIA. LOS TOKENS DE MAINNET DE CELESTIA ACTUALMENTE NO EXISTEN NI SE HACEN VENTAS PÚBLICAS U OTRAS DISTRIBUCIONES PÚBLICAS DE CUALQUIER TOKEN DE MAINNET DE CELESTIA.

Puedes solicitar desde Mamaki Testnet Faucet en el canal #faucet en el servidor de Discord de Celestia con el siguiente comando:

```text
$request <CELESTIA-ADDRESS>
```

Donde `<CELESTIA-ADDRESS>` es una `celestia1******` dirección generada.

> Nota: Faucet tiene un límite de 10 tokens por semana por dirección/Discord ID

## Exploradores

Hay varios exploradores que puedes usar para Mamaki:

* [https://testnet.mintscan.io/celestia-testnet](https://testnet.mintscan.io/celestia-testnet)
* [https://celestia.explorers.guru/](https://celestia.explorers.guru/)
* [https://celestiascan.vercel.app/](https://celestiascan.vercel.app/)

## Configurar la red P2P

Ahora configuraremos las redes P2P clonando el repositorio de redes:

```sh
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git
```

Para inicializar la red elige un "node-name" que describa tu nodo. El parámetro --chain-id que estamos usando aquí es `mamaki`. Ten en cuenta que esto podría cambiar si se despliega una nueva testnet.

```sh
celestia-appd init "node-name" --chain-id mamaki
```

Copie el archivo `genesis.json`. Para mamaki estamos usando:

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

Establecer seeds y peers:

<!-- markdownlint-disable MD013 -->
```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml
```
<!-- markdownlint-enable MD013 -->

Nota: Puedes encontrar más peers [aquí](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt).

## Sincronización rápida con snapshot

Ejecuta el siguiente comando para sincronizar rápidamente desde un snapshot para `mamaki`:

```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```

## Delegar a un validador

Para delegar tokens al validador de `celestiavaloper`, como un ejemplo puedes ejecutar:

```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```

Si se ha ejecutado con éxito, deberías ver una salida similar a esta:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Puedes comprobar si el hash TX fue enviado usando el explorador de bloques ingresando el ID de `txhash` que fue devuelto.

## Conectar validador

Continuando con el tutorial Validator, aquí están los pasos para conectar tu validador a Mamaki:

```sh
MONIKER="tu_moniker"
VALIDATOR_WALLET="validator"

celestia-appd tx staking create-validator \
    --amount=1000000utia \
    --pubkey=$(celestia-appd tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=mamaki \
    --commission-rate=0.1 \
    --commission-max-rate=0.2 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1000000 \
    --from=$VALIDATOR_WALLET \
    --keyring-backend=test
```

Se te pedirá que confirmes la transacción:

```console
confirm transaction before signing and broadcasting [y/N]: y
```

La entrada `y` debe proporcionar una salida similar a:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Ahora deberías poder ver a tu validador desde un explorador de bloques como [aquí](https://celestia.explorers.guru/)
