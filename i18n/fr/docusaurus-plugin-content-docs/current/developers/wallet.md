- - -
sidebar_label : Création d'un Wallet
- - -

# Wallet

## Créer un Wallet (Portefeuille)

Tout d'abord, créez un fichier de configuration CLI de l'application :

```sh
celestia-appd config keyring-backend test
```

Vous pouvez choisir le nom de Wallet que vous voulez. Pour notre exemple, nous avons utilisé "validator" comme nom de wallet :

```sh
celestia-appd keys add validator
```

Enregistrez la sortie mnémonique car c'est la seule façon de récupérer votre wallet de validateur en cas de perte !

Pour vérifier tous vos portefeuilles, vous pouvez exécuter :

```sh
celestia-appd keys list
```

## Financer un Wallet

For the public celestia address, you can fund the previously created wallet via [Discord](https://discord.gg/celestiacommunity) by sending this message to #arabica-faucet channel:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Attendez de voir si vous obtenez une confirmation que les tokens (jetons) ont été envoyés avec succès. Pour vérifier si les tokens sont arrivés avec succès à destination du wallet exécutez la commande ci-dessous en remplaçant l'adresse publique avec la vôtre :

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
