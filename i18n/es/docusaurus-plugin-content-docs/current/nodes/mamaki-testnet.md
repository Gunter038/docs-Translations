- - -
sidebar_label : Mamaki Testnet
- - -

# Mamaki Testnet
<!-- markdownlint-disable MD013 -->

![mamaki-testnet](/img/mamaki.png)

Esta guía contiene las secciones relevantes sobre cómo conectarse a Mamaki, dependiendo del tipo de nodo que esté ejecutando. Mamaki es un hito en Celestia, permitiendo a todos probar las funcionalidades principales de la red. Lee el anuncio [aquí](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/).

Tu mejor enfoque para participar es determinar primero qué nodo deseas ejecutar. Cada guía de nodo se enlazará a la red relevante para mostrarte cómo conectarse a ellos.

Tienes una lista de opciones sobre el tipo de nodos que puedes ejecutar para participar en Mamaki:

Consensos:

* [Nodo Validador](./validator-node.md)
* [Full Node](./consensus-full-node.md)

Nodos de disponibilidad de datos:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Selecciona el tipo de nodo que deseas ejecutar y sigue las instrucciones en cada página correspondiente. Cada vez que se te pida que selecciones el tipo de red a la que desea conectarse en esas guías, selecciona `Mamaki` para referir a las instrucciones correctas en esta página sobre cómo conectarse a Mamaki.

## Endpoints RPC

Hay una lista de puntos finales RPC que puedes utilizar para conectarte a Mamaki Testnet:

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://grpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)

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

Copie el archivo `genesis.json`. For mamaki we are using:

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

Set seeds and peers:

```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml

```

Note: You can find more peers [here](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt).

## Quick-sync with snapshot

Run the following command to quick-sync from a snapshot for `mamaki`:

```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```

## Delegate to a validator

To delegate tokens to the the `celestiavaloper` validator, as an example you can run:

```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```

If successful, you should see a similar output as:

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

You can check if the TX hash went through using the block explorer by inputting the `txhash` ID that was returned.

## Connect validator

Continuing the Validator tutorial, here are the steps to connect your validator to Mamaki:

```sh
MONIKER="your_moniker"
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

You will be prompted to confirm the transaction:

```console
confirm transaction before signing and broadcasting [y/N]: y
```

Inputting `y` should provide an output similar to:

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

You should now be able to see your validator from a block explorer like [here](https://celestia.explorers.guru/)
