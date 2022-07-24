- - -
sidebar_label : Créer un Testnet Celestia
- - -

# Guide d'instanciation du réseau Celestia App

Ce guide est destiné à aider à instancier un nouveau réseau de test et à suivre les étapes correctes pour le faire avec Celestia-App. Vous ne devriez suivre que ce guide si vous voulez expérimenter avec votre propre réseau test Celestia ou si vous voulez tester de nouvelles fonctionnalités à développer en tant que développeur.

## Hardware Requis

Vous pouvez suivre les exigences matérielles [ici](../nodes/validator-node.md#hardware-requirements).

## Configurer les dépendances

Vous pouvez configurer les dépendances en suivant le guide [ici](./environment.md).

## Installation de Celestia App

Vous pouvez installer Celestia App en suivant le guide [ici](./celestia-app.md).

## Spin Up A Celestia Testnet

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

Une fois tous les validateurs ajoutés, le fichier `genesis.json` est créé. You need to share it with all other validators in your testnet in order for everyone to proceed with the following step.

You can find the `genesis.json` at `$HOME/.celestia-appd/config/genesis.json`

### Create the Genesis Transaction For New Chain

Run the following command:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

This will create the genesis transaction for your new chain. Here `$STAKING_AMOUNT` should be at least `1000000000utia`. If you provide too much or too little, you will encounter an error when starting your node.

You will find the generated gentx JSON file inside `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Note: If you have other validators in your network, they need to also run the above command with the `genesis.json` file you shared with them in the previous step.

### Creating the Genesis JSON File

Once all participants have submitted their gentx JSON files to you, you will pull all those gentx files inside the following directory: `$HOME/.celestia-appd/config/gentx` and use them to create the final `genesis.json` file.

Once you added the gentx files of all the particpants, run the following command:

```sh
celestia-appd collect-gentxs 
```

This command will look for the gentx files in this repo which should be moved to the following directory `$HOME/.celestia-app/config/gentx`.

It will update the `genesis.json` file after in this location `$HOME/.celestia-app/config/genesis.json` which now includes the gentx of other participants.

You should then share this final `genesis.json` file with all the other particpants who must add it to their `$HOME/.celestia-app/config` directory.

Everyone must ensure that they replace their existing `genesis.json` file with this new one created.

### Modify Your Config File

Open the following file `$HOME/.celestia-app/config/config.toml` to modify it.

Inside the file, add the other participants by modifying the following line to include other participants as persistent peers:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

You can find `validator_address` by running the following command:

```sh
celestia-appd tendermint show-node-id
```

The output will be the hex-encoded `validator_address`. The default `port` is 26656.

### Instantiate the Network

You can start your node by running the following command:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
