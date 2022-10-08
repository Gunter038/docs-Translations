- - -
sidebar_label : Créer un Testnet Celestia
- - -

# Guide d'instanciation du réseau Celestia App

Ce guide est destiné à aider à instancier un nouveau réseau de test et à suivre les étapes correctes pour le faire avec Celestia-App. Vous ne devriez suivre ce guide que si vous voulez expérimenter avec votre propre réseau test Celestia ou si vous voulez tester de nouvelles fonctionnalités à développer en tant que développeur.

## Hardware Requis

Vous pouvez suivre les exigences matérielles [ici](../nodes/validator-node.md#hardware-requirements).

## Configurer les dépendances

Vous pouvez configurer les dépendances en suivant le guide [ici](./environment.md).

## Installation de Celestia App

Vous pouvez installer Celestia App en suivant le guide [ici](./celestia-app.md).

## Faire tourner un testnet sur Celestia

Si vous souhaitez créer un réseau test rapide avec vos amis, vous pouvez suivre ces étapes. Sauf indication contraire, chaque étape doit être effectuée par tous ceux qui veulent participer à ce réseau test.

### Optionnel: Réinitialiser le répertoire de travail

Si vous avez déjà initialisé un répertoire de travail pour `celestia-appd` dans le passé, vous devez le nettoyer avant de réinitialiser un nouveau répertoire. Vous pouvez le faire en exécutant la commande suivante :

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Initialiser un répertoire de travail

Exécutez la commande suivante :

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* La valeur que nous utiliserons pour `$VALIDATOR_NAME` est `validator1` mais vous devriez choisir votre propre nom de node.
* La valeur que nous utiliserons pour `$CHAIN_ID` est `testnet`. La `$CHAIN_ID` doit rester la même pour tous ceux qui participent à ce réseau.

### Créer une nouvelle clé

Ensuite, exécutez la commande suivante:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

Cela créera une nouvelle clé, avec un nom de votre choix. Enregistrez la sortie de cette commande quelque part ; vous aurez besoin de l'adresse générée ici plus tard. Ici, nous fixons la valeur de notre clé `$KEY_NAME` à `validator` pour la démonstration.

### Ajouter le nom de clé du compte Genesis

Exécutez la commande suivante :

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Ici `$VALIDATOR_NAME` est le même nom de clé qu'avant ; et `$AMOUNT` est quelque chose comme `10000000000000000000000000utia`.

### Facultatif : Ajouter d'autres validateurs

Si d'autres participants de votre réseau test veulent également être des validateurs, répétez la commande ci-dessus avec le montant spécifique pour leurs clés publiques.

Une fois tous les validateurs ajoutés, le fichier `genesis.json` est créé. Vous avez besoin de le partager avec tous les autres validateurs de votre testnet afin que tout le monde puisse passer à l'étape suivante.

Vous pouvez trouver le `genesis.json` dans `$HOME/.celestia-app/config/genesis.json`

### Créer la transaction Genesis pour la nouvelle chaîne

Exécutez la commande suivante :

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

Cela créera la transaction Genesis pour votre nouvelle chaîne. Ici, `$STAKING_AMOUNT` doit être au moins égal à `1000000000utia`. Si vous fournissez trop ou trop peu, vous rencontrerez une erreur lors du démarrage de votre Node.

Vous trouverez le fichier JSON gentx généré dans `$HOME/.celestia-
app/config/gentx/gentx-$KEY_NAME.json`

> Remarque : Si vous avez d'autres validateurs dans votre réseau, ils doivent également exécuter la commande ci-dessus avec le fichier `genesis.json` que vous avez partagé avec eux à l'étape précédente.

### Création du fichier JSON Genesis

Une fois que tous les participants vous ont soumis leurs fichiers gentx JSON, vous allez mettre tous ces fichiers gentx dans le répertoire suivant : `$HOME/. elestia-appd/config/gentx` et utilisez-les pour créer le fichier final `genesis.json`.

Une fois que vous avez ajouté les fichiers gentx de tous les particpants, lancez la commande suivante :

```sh
celestia-appd collect-gentxs
```

Cette commande va rechercher les fichiers gentx de ce dépôt qui devraient être déplacés dans le répertoire suivant `$HOME/.celestia-app/config/gentx`.

Elle mettra à jour le fichier `genesis.json` dans cet emplacement `$HOME/. elestia-app/config/genesis.json` qui inclut maintenant le gentx des autres participants.

Vous devriez ensuite partager ce fichier `genesis.json` final avec tous les autres participants qui doivent l'ajouter à leur dossier `$HOME/.celestia-app/config`.

Tout le monde doit s'assurer de remplacer le fichier `genesis.json` existant par ce nouveau fichier créé.

### Modifiez votre fichier de configuration

Ouvrez le fichier suivant `$HOME/.celestia-app/config/config.toml` pour le modifier.

À l'intérieur du fichier, ajoutez les autres participants en modifiant la ligne suivante pour inclure d'autres participants en tant que pairs persistants:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

Vous pouvez trouver la `validator_address` en exécutant la commande suivante:

```sh
celestia-appd tendermint show-node-id
```

La sortie sera le code hexadécimal `validator_address`. Par défaut, le `port` est 26656.

### Instancier le réseau

Vous pouvez commencer à synchroniser votre node en exécutant la commande suivante :

```sh
celestia-appd start
```

Maintenant vous avez un nouveau réseau test Celestia avec lequel jouer !
