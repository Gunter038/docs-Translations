---
sidebar_label: Exécuter la chaine Wordle
---

# Exécuter la chaine Wordle
<!-- markdownlint-disable MD013 -->

## Construire et exécuter la chaine Wordle

Dans un terminal, exécuter la commande suivante :

```sh
ignite chain serve 
```

Cela va compiler le code de la blockchain que vous venez d'écrire, créer un fichier genesis et quelques comptes à utiliser. Lorsque le registre affiche quelque chose similaire à ce qui vient, entrer ceci :

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5

🛠️  Construire un proto...
📦 Installer des dépendances...
🛠️  Building the blockchain...
💿 Initialiser l'application...
🙂 Created account "alice" with address "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" with mnemonic: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Created account "bob" with address "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" with mnemonic: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Ici la commande a créé un binaire appelé `wordled` et des adresses `alice` et `bob` avec un faucet et un API. Vous êtes libre de quitter le programme avec la fonction CTRL-C. La raison étant que nous allons exécuter séparément le binaire `wordled` avec les drapeaux Optimint intégrés.

Vous pouvez commencer la chaine avec des configurations Optimint en exécutant la commande suivante :

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Remarques :

> NOTE : dans la commande ci-dessus, vous avez besoin de fournir une adresse IP d'un nœud Celestia dont le compte est approvisionné en tokens devnet Arabica à la `base_url`. Nous vous invitons à suivre le tutoriel pour configurer un light node Celestia et créer un portefeuille avec des tokens faucet de testnet, dans la section Nœud Celestia [ici](./node-tutorial.md).

Remarques complémentaires :

> IMPORTANT : Il est également important dans la commande ci-dessus d'identifier la dernière position du bloc dans le devnet Arabica par `da_height`. Vous pouvez trouver le numéro du dernier bloc dans l'explorateur [ici](https://explorer.celestia.observer/arabica). Enfin, pour le drapeau `--optimint.namespace_id`, vous pouvez générer un identifiant aléatoire d'espace de nom (Namespace) en utilisant l'environnement de test [ici](https://go.dev/play/p/7ltvaj8lhRl)

Dans une autre fenêtre, exécutez la commande suivante pour soumettre un Wordle :

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async -y
```

> NOTE : Nous soumettons une transaction asynchrone car nous souhaitons éviter les erreurs de timeout. Optimint remplaçant Tendermint, nous avons besoin d'attendre le réseau de disponibilité des données de Celestia pour nous assurer qu'un bloc a été inclus dans Wordle, avant de passer au bloc suivant. Actuellement dans Optimint, l'agrégateur unique n'avance pas avec le bloc suivant en production tant qu'il tente de soumettre le bloc antérieur au réseau de disponibilité des données. Dans le futur, avec la leader election, la production du bloc et la logique de synchronisation vont s'améliorer considérablement.

Cela vous demandera de confirmer la transaction avec le message suivant :

```json
{
  "body":{
    "messages":[
       {
          "@type":"/YazzyYaz.wordle.wordle.MsgSubmitWordle",
          "creator":"cosmos17lk3fgutf00pd5s8zwz5fmefjsdv4wvzyg7d74",
          "word":"giant"
       }
    ],
    "memo":"",
    "timeout_height":"0",
    "extension_options":[
    ],
    "non_critical_extension_options":[
    ]
  },
  "auth_info":{
    "signer_infos":[
    ],
    "fee":{
       "amount":[
       ],
       "gas_limit":"200000",
       "payer":"",
       "granter":""
    }
  },
  "signatures":[
  ]
}
```

Le Cosmos-SDK vous demandera alors de confirmer la transaction ici :

```sh
confirm transaction before signing and broadcasting [y/N]:
```

Confirmez en tapant un Y.

Vous aurez alors une réponse avec un hash de transaction comme indiqué ci-dessous :

```sh
code: 19
codespace: sdk
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128
```

Notez que cela ne signifie pas que la transaction était déjà inclue dans le bloc. Demandons le hash de la transaction pour vérifier s'il a été inclus dans le bloc ou s'il y a des erreurs.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

Cela devrait afficher le résultat suivant :

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Essayons quelques petites choses pour nous amuser :

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

Après avoir confirmé la transaction, interrogez le `txhash` de la même manière que vous l'avez fait ci-dessus. Vous verrez que la réponse sera "Erreur Invalide" car vous avez soumis des entiers.

Maintenant essayez la commande suivante :

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

Après avoir confirmé la transaction, interrogez le `txhash` de la même manière que vous l'avez fait ci-dessus. Vous verrez que la réponse sera une "Erreur Invalide" à nouveau car vous avez soumis un mot de plus de 5 caractères.

Maintenant essayez de soumettre un autre mot même si l'un d'eux a déjà été soumis

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

Après avoir confirmé la transaction, interrogez le `txhash` de la même manière que vous l'avez fait ci-dessus. Vous allez avoir une erreur vous disant qu'un wordle a déjà été soumis pour aujourd'hui.

Maintenant essayons de deviner un mot à cinq lettres :

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

Après avoir soumis les transactions et les avoir confirmées, interroger le `txhash` de la même manière que vous l'avez fait ci-dessus. Etant donné que vous n'avez pas deviné le bon mot, cela augmentera le nombre de guess du compte Bob.

Nous pouvons vérifier cela en interrogeant la liste :

```sh
wordled q wordle list-guess --output json
```

Cela fournit tous les guess soumis jusqu'à présent, l'index étant la date d'aujourd'hui et l'adresse de l'émetteur.

Grâce à cela, nous avons implémenté un exemple basique de Wordle en utilisant le Cosmos-SDK, Ignite et Optimint. La prochaine partie sera sur la façon d'étendre la codebase.

## Étendre dans le futur

Vous pouvez étendre la code base et améliorer ce tutoriel en consultant le dossier github [ici](https://github.com/celestiaorg/wordle).

Il y a de multiples façons d'étendre cette codebase :

1. Vous pouvez améliorer le message lorsque vous devinez le mot correct.
2. Vous pouvez hash le mot avant de le soumettre à la chaîne en vous assurant que le hashing est local et qu'il ne soit pas révélé en front-run par d'autres qui surveillent la chaîne pendant qu'il est soumis à celle-ci.
3. Vous pouvez améliorer l'interface utilisateur dans le terminal en utilisant une interface plus jolie pour Wordle. Quelques exemples sont proposés [ici](https://github.com/nimblebun/wordle-cli).
4. Vous pouvez améliorer la date actuelle pour qu'elle soit adaptée à un fuseau horaire spécifique.
5. Vous pouvez créer un bot qui soumet un mot chaque jour à une heure spécifique.
6. Vous pouvez créer une vue.js en front-end avec Ignite en utilisant les ressources en accès libre [ici](https://github.com/yyx990803/vue-wordle) and [ici](https://github.com/xudafeng/wordle).
