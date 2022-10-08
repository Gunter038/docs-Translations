- - -
sidebar_label : Node Validateur
- - -

# Configurer un Node validateur

Les nodes Validateurs vous permettent de participer au consensus sur le réseau de Celestia.

## Hardware Requis

Les exigences matérielles minimales suivantes sont recommandées pour exécuter le node validateur : :

* Mémoire : 8 Go de RAM
* CPU : Quad-Core
* Disque : 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configuration de votre Node validateur

Le tutoriel suivant est fait sur une machine d'instance Linux Ubuntu 20.04 (LTS) x64.

### Configurer les dépendances

Suivez le tutoriel ici pour installer les dépendances [ici](../developers/environment.md).

## Déployer Celestia-App

Cette section décrit la première partie de l'installation d'un Node Validateur Celestia: exécutant un daemon Celestia App avec un node Celestia Core interne.

> Remarque : Assurez-vous de disposer d'au moins 100 Go d'espace de libre pour installer et exécuter en toute sécurité le node validateur .

### Installer celestia-app

Suivez le tutoriel d'installation de Celestia App [ici](../developers/celestia-app.md).

### Configurer les réseaux pair-à-pair

Pour cette section du guide, sélectionnez le réseau auquel vous souhaitez vous connecter :

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Après cela, vous pouvez continuer avec le reste du tutoriel.

### Configurer pruning

Pour une utilisation réduite de l'espace disque, nous vous recommandons de configurer le pruning à l'aide de la configurations ci-dessous. Vous pouvez changer cela pour vos propres configurations de pruning, si vous le désirez:

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

### Configurer le Node Validateur

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### Réinitialiser le réseau

Cela supprimera tous les dossiers de données afin de pouvoir recommencer à zéro :

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Optionnel : synchronisation rapide avec snapshot

La synchronisation depuis le Genesis peut prendre beaucoup de temps, selon votre matériel. Utiliser cette méthode vous permet de synchroniser votre node Celestia très rapidement en téléchargeant un Snapshot récent de la blockchain. Si vous souhaitez synchroniser depuis le Genesis, alors vous pouvez ignorer cette partie.

Si vous souhaitez utiliser un Snapshot, déterminez le réseau auquel vous souhaitez vous synchroniser à partir de la liste ci-dessous :

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Démarrer Celestia-App avec SystemD

Suivre le tutoriel pour configurer la Celestia App en tant que processus de fond avec SystemD [ici](./systemd.md#start-the-celestia-app-with-systemd).

### Wallet

Suivez le tutoriel sur la création d'un wallet [ici](../developers/wallet.md).

### Déléguer de la Stake (mise) à un validateur

Créer une variable d'environnement pour l'adresse:

```sh
VALIDATOR_WALLET=<validator-address>
```

Si vous souhaitez déléguer plus de stake à n'importe quel validateur, y compris le vôtre, vous aurez besoin de l'adresse `celesvaloper` du validateur en question. Vous pouvez soit la vérifier en utilisant l'explorateur de blocs mentionné ci-dessus ou vous pouvez exécuter la commande ci-dessous pour obtenir le `celesvaloper` de votre wallet de validateur local dans le cas où vous souhaitez lui déléguer davantage :

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

Après avoir entré la passphrase du portefeuille, vous devriez voir une sortie similaire :

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

Ensuite, sélectionnez le réseau que vous souhaitez utiliser pour déléguer à un validateur :

* [Mamaki](./mamaki-testnet.md#delegate-to-a-validator)

## Deployer le Celestia Node

Cette section décrit la partie 2 de l'installation d'un node validateur Celestia : exécuter le daemon Celestia Bridge Node.

### Installer Celestia Node

Vous pouvez suivre le tutoriel d'installation de Celestia Node [ici](../developers/celestia-node.md)

### Initialiser le Bridge node

Exécutez ce qui suit :

```sh
celestia bridge init --core.ip <ip-address> --core.grpc.port <port>
```

> NOTE : Le port `--core.grpc.port` est configuré par défaut à 9090, donc si vous n'en spécifiez pas un autre dans la ligne de commande, il s'exécutera sur celui-là par défaut. Vous pouvez utiliser le drapeau (flag) pour spécifier un autre port si vous préférez.

Si vous avez besoin d'une liste de terminaux RPC pour vous connecter, vous pouvez vérifier dans la liste [ici](./mamaki-testnet.md#rpc-endpoints)

### Exécuter le Bridge node

Exécutez ce qui suit :

```sh
celestia bridge start
```

### Facultatif : Démarrez le Bridge Node avec SystemD

Suivez le tutoriel sur la configuration du Bridge node en tant que processus de fond avec SystemD [ici](./systemd.md#celestia-bridge-node).

Vous avez configuré avec succès un Bridge node qui se synchronise avec le réseau.

## Exécuter un Node Validateur

Après avoir terminé toutes les étapes nécessaires, vous êtes maintenant prêt à exécuter un validateur ! Afin de créer votre validateur sur la chaîne, suivez les instructions ci-dessous. Gardez à l'esprit que ces étapes sont nécessaires UNIQUEMENT si vous souhaitez participer au consensus.

Choisissez un nom de `moniker` de votre choix! C'est le nom du validateur qui s'affichera sur les tableaux de bord publics et les explorateurs. `VALIDATOR_WALLET` doit être le même que celui défini précédemment. Le paramètre `--min-self-delegation=1000000` définit la quantité de tokens qui sont auto-délégués depuis votre wallet de validateur.

Maintenant, connectez-vous au réseau de votre choix.

Vous avez l'option suivante de vous connecter à la liste des réseaux montrée ci-dessous:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Complétez les instructions du réseau que vous souhaitez valider pour terminer le processus de configuration du validateur.
