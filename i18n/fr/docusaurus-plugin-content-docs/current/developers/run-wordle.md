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

Here the command created a binary called `wordled` and the `alice` and `bob` addresses, along with a faucet and API. You are clear to exit the program with CTRL-C. The reason for that is because we will run `wordled` binary separately with Optimint flags added.

Vous pouvez commencer la chaine avec des configurations Optimint en exécutant la commande suivante :

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Remarques :

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Arabica devnet tokens. Follow the tutorial for setting up a Celestia Light Node and creating a wallet with testnet faucet money [here](./node-tutorial.md) in the Celestia Node section.

Remarques complémentaires :

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Arabica Devnet for `da_height`. You can find the latest block number in the explorer [here](https://explorer.celestia.observer/arabica). Also, for the flag `--optimint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

In another window, run the following to submit a Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

> NOTE: We are submitting a transaction asynchronously due to avoiding any timeout errors. With Optimint as a replacement to Tendermint, we need to wait for Celestia's Data-Availability network to ensure a block was included from Wordle, before proceeding to the next block. Currently, in Optimint, the single aggregator is not moving forward with the next block production as long as it is trying to submit the current block to the DA network. In the future, with leader selection, block production and sync logic improves dramatically.

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

Notez que cela ne signifie pas que la transaction était déjà inclue dans le bloc. Let's query the transaction hash to check whether it has been included in the block yet or if there are any errors.

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

After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted integers.

Maintenant essayez la commande suivante :

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted a word larger than 5 characters.

Maintenant essayez de soumettre un autre mot même si l'un d'eux a déjà été soumis

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. You will get an error that a wordle has already been submitted for the day.

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

Grâce à cela, nous avons implémenté un exemple basique de Wordle en utilisant le Cosmos-SDK, Ignite et Optimint. La prochaine partie sera sur la façon d'étendre la code base.

## Étendre dans le futur

Vous pouvez étendre la code base et améliorer ce tutoriel en consultant le dossier github [ici](https://github.com/celestiaorg/wordle).

Il y a de multiples façons d'étendre cette code base :

1. Vous pouvez améliorer le message lorsque vous devinez le mot correct.
2. You can hash the word prior to submitting it to the chain, ensuring the hashing is local so that it’s not revealed via front-running by others monitoring the plaintext string when it’s submitted on-chain.
3. Vous pouvez améliorer l'interface utilisateur dans le terminal en utilisant une interface plus jolie pour Wordle. Quelques exemples sont proposés [ici](https://github.com/nimblebun/wordle-cli).
4. Vous pouvez améliorer la date actuelle pour qu'elle soit adaptée à un fuseau horaire spécifique.
5. Vous pouvez créer un bot qui soumet un mot chaque jour à une heure spécifique.
6. You can create a vue.js front-end with Ignite using example open-source repositories [here](https://github.com/yyx990803/vue-wordle) and [here](https://github.com/xudafeng/wordle).
