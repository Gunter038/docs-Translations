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

> CATATAN: Kita mengirim transaksi secara tidak sinkron yang disebabkan setiap kesalahan batas waktu. Dengan Rollmint sebagai pengganti Tendermint, kita butuh untuk menunggu jaringan Ketersediaan Data Celestia untuk memastikan block yang sudah masuk dalam Wordle, sebelum diproses ke block selanjutnya. Saat ini, di Rollmint, agregator tunggal tidak bergerak maju dengan produksi block selanjutnya selama percobaan pengiriman saat ini ke jaringan DA. Dimasa depan, dengan pilihan pemimpin, produksi block dan meningkatkan logika sinkron secara dramatis.

Ini akan memintamu mengkonfirmasi transaksi dengan pesan berikut:

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

Cosmos-SDK memintamu untuk mengkonfirmasi transaksi disini:

```sh
konfirmasi transaksi sebelum masuk dan disiarkan [y/N]:
```

Konfirmasi dengan Y.

Kamu akan mendapatkan respon dengan hash transaksi yang muncul disini:

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

Catatan, ini tidak berarti transaksi telah masuk dalam block. Mari query hash transksi untuk memeriksa apakah sudah masuk dalam block atau disana ada beberapa kesalahan.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

Ini yang ditampilkan harus keluar seperti berikut:

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Uji coba beberapa hal untuk kesenangan:

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

Setelah konfirmasi transaksi, query `txhash` diberikan cara sama yang kamu lalukan diatas. Kamu akan melihat respon yang menunjukkan kesalahan tak valid karena kamu mengirim bilangan bulat.

Sekarang coba:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

Setelah konfirmasi transaksi, query `txhash` diberikan dengan cara sama yang kamu lakukan diatas. Kamu akan melihat respon menunjukkan kesalahan tak valid karena kamu mengirim huruf lebih dari 5 karakter.

Sekarang coba untuk mengirim wordle lainnya selain salah satu yang telah dikirim

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

Setelah mengirim transaksi dan mengkonfirmasinya, query `txhash` diberikan dengan cara sama yang kamu lakukan diatas. Kamu akan dapat sebuah kesalahan pada wordle yang telah dikirim untuk hari ini.

Sekarang mari coba untuk menebak lima huruf kata:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

Setelah mengirim transaksi dan mengkonfirmasinya, query <0>txhash</0> diberikan dengan cara sama yang kamu lakukan diatas. Memberikanmu tebakan kata yang salah, ini akan menambah jumlah tebakan akun Bob.

Kita dapat memverifikasi ini dengan daftar query:

```sh
wordled q wordle list-guess --output json
```

Ini hasil semua objek tebakan yang dikirim sampai saat ini, dengan indeks berwujud tanggal hari dan alamat pengirim.

Dengan itu, kita mengimplementasikan contoh dasar dari Wordle menggunakan Cosmos_SDK dan Ignite dan Rollmint. Baca lebih lanjut bagaimana kamu dapat memperluas basis kode.

## Perluasan dimasa depan

Kamu dapat memperluas basis data dan meningkatkan tata cara ini dengan memeriksa repositori yang ada [disini](https://github.com/celestiaorg/wordle).

Disana banyak cara kode basis ini dapat diperluas:

1. Kamu dapat meningkatkan perpesanan antara dimana kamu menebak kata yang benar.
2. Kamu dapat hash kata utama untuk dikirim ke chain, pastikan hashing secara lokal jadi itu tidak akan terbuka melalui front yang berjalan dengan pemantau string teks biasa lainnya ketika itu dikirim ke chain.
3. Kamu dapat meningkatkan UI di terminal mengunakan tata letak yang bagus untuk Wordle. Beberapa contoh [disini](https://github.com/nimblebun/wordle-cli).
4. Kamu dapat meningkatkan tanggal saat ini untuk menetapkan zona waktu spesifik.
5. Kamu dapat membuat bot yang mengirim wordle setiap hari di jam tertentu.
6. Kamu dapat membuat vue.js front-end dengan Ignite menggunakan repositori open-source contoh [disini](https://github.com/yyx990803/vue-wordle) dan [disini](https://github.com/xudafeng/wordle).
