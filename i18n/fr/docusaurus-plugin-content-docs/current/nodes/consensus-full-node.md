- - -
sidebar_label : Full Node de Consensus
- - -

# Configurer un Full Node de Consensus de Celestia
<!-- markdownlint-disable MD013 -->

Les Full Nodes vous permettent de synchroniser l'historique de la blockchain dans la couche de consensus de Celestia.

## Hardware Requis

La configuration matérielle minimale requise pour exécuter le full node de consensus est la suivante :

* Mémoire vive : 8Go de RAM
* Processeur : 4 cœurs
* Disque dur : 250 Go de stockage SSD
* Bande passante : 1 Gbps pour le téléchargement/100 Mbps pour l'upload

## Configurer votre full node de consensus

Le tutoriel ci-après a été réalisé sur un système d'exploitation Ubuntu Linux 20.04 (LTS) x64.

### Configurer les dépendances

Suivre les instructions pour installer les dépendances [ici](../developers/environment.md).

## Déployer Celestia-App

Cette section décrit la partie 1 de la configuration d'un full node de consensus Celestia : exécuter un daemon Celestia App avec un Node interne Celestia Core.

> Note : vérifier que vous avez au moins 100+ Go d'espace libre pour installer et exécuter en tout sécurité le full node de consensus.

### Installer celestia-app

Suivre le tutoriel pour installer la Celestia App [ici](../developers/celestia-app.md).

### Configurer les réseaux pair-à-pair

Pour cette section du guide, sélectionner le réseau que vous voulez connecter à :

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Après cela, vous pouvez continuer le tutoriel.

### Configurer pruning

Pour des disques durs aux espaces de stockage plus réduits nous recommandons de configurer pruning en utilisant les paramètres ci dessous. Vous pouvez utiliser vos propres paramètres de pruning si vous voulez :

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

### Réinitialiser le réseau

Cela va supprimer tous les dossiers de données pour pouvoir recommencer à zéro :

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Optionnel : synchronisation rapide avec snapshot

Synchroniser à partir du Genesis peut prendre beaucoup de temps, selon votre matériel. En utilisant cette méthode vous synchronisez très rapidement votre node Celestia en téléchargeant un snapshot récent de la blockchain. Si vous souhaitez synchroniser à partir du Genesis vous pouvez ignorer cette partie.

Si vous souhaitez utiliser le snapshot, déterminez à quel réseau vous souhaitez vous synchroniser à partir de la liste ci-dessous :

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Lancer celestia-app

Afin de démarrer votre full node de consensus, exécutez la commande ci-dessous :

```sh
celestia-appd start
```

Cela vous permettra de synchroniser l'historique de la blockchain Celestia.

### Optionnel: configuration du point de terminaison RPC

Vous pouvez configurer votre Full Node de consensus pour qu'il soit un terminal RPC public et écouter toutes les connexions des nodes de Disponibilité de Données afin de servir les requêtes de l'API de Disponibilité de Données [ici](../developers/node-tutorial.md).

Notez que vous devez vous assurer que le port 9090 est ouvert pour cela.

Exécutez les commandes suivantes :

```sh
EXTERNAL_ADDRESS=$(wget -qO- eth0.me)
sed -i.bak -e "s/^external-address = \"\"/external-address = \"$EXTERNAL_ADDRESS:26656\"/" $HOME/.celestia-app/config/config.toml
sed -i 's#"tcp://127.0.0.1:26657"#"tcp://0.0.0:26657"#g' ~/.celestia-app/config/config/config.toml
```

Redémarrez `celestia-appd` à l'étape précédente pour charger ces configurations.

### Démarrer Celestia-App avec SystemD

Il est possible de suivre le tutoriel pour configurer Celestia-App en tant que processus de fond avec SystemD, [ici](./systemd.md#start-the-celestia-app-with-systemd).
