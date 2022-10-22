---
sidebar_label: Jalankan Chain Wordle
---

# Jalankan Chain Wordle
<!-- markdownlint-disable MD013 -->

## Bangun dan Jalankan Chain Wordle

Di windows terminal, jalankan perintah berikut:

```sh
ignite chain serve 
```

Ini akan menyatukan kode blockchain kamu hanya menulis dan juga membuat file genesis dan beberapa akun untuk kamu gunakan. Sesudah log menampilkan yang keluar seperti log berikut:

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5

🛠️  Building proto...
📦 Installing dependencies...
🛠️  Building the blockchain...
💿 Initializing the app...
🙂 Created account "alice" with address "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" with mnemonic: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Created account "bob" with address "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" with mnemonic: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Disini perintah dibuat binary yang disebut `wordled` dan alamat `alice` and `bob`, serta dengan faucet dan API. Pastikan kamu menutup program dengan CTRL-C. Alasannya untuk itu karena kita akan menjalankan binary `wordled` secara terpisah dengan bendera Rollmint yang ditambahkan.

Kamu dapat memulai chain dari konfigurasi rollmint dengan menjalankan berikut:

```sh
wordled start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```

Mohon pertimbangkan:

> CATATAN: Perintah diatas, kamu membutuhkan melewati alamat IP Node Celestia untuk `base_url` akun yang telah dimiliki dengan token devnet Arabica. Ikuti tata cara untuk mengatur Node Light Celestia dan membuat dompet dengan uang faucet testnet [disini](./node-tutorial.md) dalam pilihan Node Celestia.

Juga mohon dipertimbangkan:

> PENTING: Selain itu, perintah diatas, kamu butuh tinggi block terakhir yang spesifik di Devnet Arabica untuk `da_height`. Kamu dapat menemukan nomor block terakhir di explorer [disini](https://explorer.celestia.observer/arabica). Juga, untuk bendera `--rollmint.namespace_id`, kamu dapat menghasilkan ID Namespace acak menggunakan permainan [disini](https://go.dev/play/p/7ltvaj8lhRl)

Di window lainnya, jalankan berikut untuk mengirim Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async -y
```

> CATATAN: Kita mengirim transaksi secara tidak sinkron yang disebabkan setiap kesalahan batas waktu. Dengan Rollmint sebagai pengganti Tendermint, kita butuh untuk menunggu jaringan Ketersediaan Data Celestia untuk memastikan block yang sudah masuk dalam Wordle, sebelum diproses ke block selanjutnya. Saat ini, di Rollmint, agregator tunggal tidak bergerak maju dengan produksi block selanjutnya selama percobaan pengiriman saat ini ke jaringan DA. In the future, with leader selection, block production and sync logic improves dramatically.

This will ask you to confirm the transaction with the following message:

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

Cosmos-SDK will ask you to confirm the transaction here:

```sh
confirm transaction before signing and broadcasting [y/N]:
```

Confirm with a Y.

You will then get a response with a transaction hash as shown here:

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

Note, this does not mean the transaction was included in the block yet. Let's query the transaction hash to check whether it has been included in the block yet or if there are any errors.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

This should display an output like the following:

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Test out a few things for fun:

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted integers.

Now try:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted a word larger than 5 characters.

Now try to submit another wordle even though one was already submitted

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. You will get an error that a wordle has already been submitted for the day.

Now let’s try to guess a five letter word:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. Given you didn’t guess the correct word, it will increment the guess count for Bob’s account.

We can verify this by querying the list:

```sh
wordled q wordle list-guess --output json
```

This outputs all Guess objects submitted so far, with the index being today’s date and the address of the submitter.

With that, we implemented a basic example of Wordle using Cosmos-SDK and Ignite and Rollmint. Read on to how you can extend the code base.

## Extending in the Future

You can extend the codebase and improve this tutorial by checking out the repository [here](https://github.com/celestiaorg/wordle).

There are many ways this codebase can be extended:

1. You can improve messaging around when you guess the correct word.
2. You can hash the word prior to submitting it to the chain, ensuring the hashing is local so that it’s not revealed via front-running by others monitoring the plaintext string when it’s submitted on-chain.
3. You can improve the UI in terminal using a nice interface for Wordle. Some examples are [here](https://github.com/nimblebun/wordle-cli).
4. You can improve current date to stick to a specific timezone.
5. You can create a bot that submits a wordle every day at a specific time.
6. You can create a vue.js front-end with Ignite using example open-source repositories [here](https://github.com/yyx990803/vue-wordle) and [here](https://github.com/xudafeng/wordle).
