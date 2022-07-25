---
sidebar_label : Creating A Wallet
---

# Wallet

## Créer un Wallet (Portefeuille)

Tout d'abord, créez un fichier de configuration d'application CLI :

 ```sh
 celestia-appd config keyring-backend test
 ```
 
Vous pouvez choisir le nom de Wallet que vous voulez.
Pour notre exemple, nous avons utilisé "validator" comme nom de wallet :

```sh
celestia-appd keys add validator
```

Enregistrez la sortie mnémonique car c'est le seul moyen de
récupérer votre Wallet validateur au cas où vous le perdriez !

Pour vérifier tous vos wallets, vous pouvez exécuter :

```sh
celestia-appd keys list
```

## Financer un Wallet

Pour l'adresse publique Celestia, vous pouvez financer le
wallet créé précédemment via Discord en envoyant
ce message au canal #faucet :

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Attendez de voir si vous obtenez une confirmation que 
les tokens (jetons) ont été envoyés avec succès. Pour vérifier si
les tokens sont arrivés avec succès à destination du 
wallet exécutez la commande ci-dessous en remplaçant l'adresse publique
avec la vôtre :

```sh
celestia-appd q bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
