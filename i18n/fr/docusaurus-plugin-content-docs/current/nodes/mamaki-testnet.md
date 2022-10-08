- - -
sidebar_label : Testnet Mamaki
- - -

# Testnet Mamaki

![mamaki-testnet](/img/mamaki.png)

Ce guide contient les sections pertinentes pour savoir comment se connecter à Mamaki, selon le type de node que vous exécutez. Le testnet Mamaki a été conçu pour aider les validateurs à essayer leur infrastructure et leur node avec le réseau de test. Les développeurs sont encouragés à déployer leurs rollups souverains sur Mamaki, mais nous recommandons également de le faire sur le devnet Arabica, qui est spécifiquement conçu à des fins de développement.

Mamaki est une étape clé de Celestia, permettant à chacun de tester les fonctionnalités de base du réseau. Lire l'annonce [ici](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/).

Votre meilleure approche pour participer est de déterminer d'abord quel nœud vous aimeriez exécuter. Chaque guide de node sera lié au réseau concerné afin de vous montrer comment vous y connecter.

Vous avez une liste d'options sur le type de nœuds que vous pouvez exécuter afin de participer à Mamaki :

Consensus:

* [Node Validateur](./validator-node.md)
* [Full Node de Consensus](./consensus-full-node.md)

Disponibilité des données :

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Sélectionnez le type de node que vous souhaitez exécuter et suivez les instructions sur chaque page respective. À chaque fois que l'on vous demande de sélectionner le type de réseau auquel vous voulez vous connecter dans ces guides, sélectionnez `Mamaki` afin de vous référer aux instructions correctes sur cette page sur la façon de vous connecter à Mamaki.

## Points de terminaison RPC

Il y a une liste des terminaux RPC que vous pouvez utiliser pour vous connecter au Testnet Mamaki:

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://grpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)

## Faucet du Testnet Mamaki

> L'UTILISATION DE CE FAUCET NE VOUS DONNERA PAS LE DROIT À UNE AUTRE DISTRIBUTION DE TOKENS DU MAINNET CELESTIA. LES JETONS DU MAINNET CELESTIA N'EXISTENT ACTUELLEMENT PAS ET IL N'Y A AUCUNE VENTE PUBLIQUE OU AUTRE DISTRIBUTION DE TOKENS DU MAINNET CELESTIA.

Vous pouvez demander des tokens au faucet du testnet Mamaki dans le canal #faucet du serveur Discord de Celestia avec la commande suivante :

```text
$request <CELESTIA-ADDRESS>
```

Où `<CELESTIA-ADDRESS>` est une adresse générée de type `celestia1******`.

> Remarque : Le faucet a une limite de 10 jetons par semaine par adresse/ID Discord

## Explorateur

Il y a plusieurs explorateurs que vous pouvez utiliser pour Mamaki:

* [https://testnet.mintscan.io/celestia-testnet](https://testnet.mintscan.io/celestia-testnet)
* [https://celestia.explorers.guru/](https://celestia.explorers.guru/)
* [https://celestiascan.vercel.app/](https://celestiascan.vercel.app/)

## Configurer le réseau pair-à-pair

Maintenant, nous allons configurer les réseaux P2P en clonant le référentiel réseau:

```sh
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git
```

Pour initialiser le réseau, choisissez un "node-name" qui décrit votre node . Le paramètre --chain-id que nous utilisons ici est `mamaki`. Garder à gardez à l'esprit que cela pourrait changer si un nouveau testnet est déployé.

```sh
celestia-appd init "node-name" --chain-id mamaki
```

Copiez le fichier `genesis.json`. Pour mamaki nous utilisons :

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

Définir les seeds et les peers:

<!-- markdownlint-disable MD013 -->
```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml
```
<!-- markdownlint-enable MD013 -->

Remarque : Vous pouvez trouver plus de peers [ici](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt).

## Synchronisation rapide avec un snapshot

Exécutez la commande suivante pour synchroniser rapidement à partir d'un snapshot pour `mamaki`:

```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```

## Déléguer à un Validateur

Pour déléguer des tokens au validateur `celestiavaloper`, par exemple, vous pouvez éxecuter :

```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```

En cas de succès, vous devriez voir une sortie similaire à:

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

Vous pouvez vérifier si le TX hash a été effectué en utilisant l'explorateur de bloc en saisissant l'ID `txhash` qui a été retourné.

## Se connecter à un Validateur

Pour continuer le tutoriel de Validateur, voici les étapes pour connecter votre validateur à Mamaki:

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

Vous serez invité à confirmer la transaction:

```console
confirm transaction before signing and broadcasting [y/N]: y
```

La saisie de `y` devrait fournir une sortie similaire à :

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

Vous devriez maintenant pouvoir voir votre validateur depuis un explorateur de blocs comme [ici](https://celestia.explorers.guru/)
