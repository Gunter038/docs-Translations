---
sidebar_label: Interaksi kontrak
---

# Interaksi Kontrak pada CosmWasm dengan Celestia
<!-- markdownlint-disable MD013 -->

Pada langkah-langkah sebelumnya, kita telah menyimpan hash tx kontrak dalam sebuah variabel lingkungan variabel lingkungan untuk digunakan nanti.

Karena periode waktu yang lebih lama untuk mengirimkan transaksi melalui Rollmint karena menunggu Lapisan Ketersediaan Data Celestia untuk mengonfirmasi penyertaan blok, kita perlu menanyakan hash tx kita secara langsung untuk mendapatkan informasi tentangnya.

## Pencarian Kontrak

Mari kita mulai dengan menanyakan hash transaksi kita untuk ID kodenya:

```sh
CODE_ID=$(wasmd query tx --type=hash $TX_HASH $NODE --output json | jq -r '.logs[0].events[-1].attributes[0].value')
echo $CODE_ID
```

Ini akan mengembalikan ID Kode dari kontrak yang digunakan.

In our case, since it's the first contract deployed on our local network, the value is `1`.

Sekarang, kita bisa melihat kontrak yang diinstansiasi oleh Code ID ini:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
```

Kita mendapatkan output berikut ini:

```json
{"contracts":[],"pagination":{"next_key":null,"total":"0"}}
```

## Instansiasi kontrak

Kita mulai menginstansiasi kontrak dengan menuliskan pesan `INIT` berikut ini untuk kontrak layanan nama. Here, we are specifying that `purchase_price` of a name is `100uwasm` and `transfer_price` is `999uwasm`.

```sh
INIT='{"purchase_price":{"amount":"100","denom":"uwasm"},"transfer_price":{"amount":"999","denom":"uwasm"}}'
wasmd tx wasm instantiate $CODE_ID "$INIT" --from $KEY_NAME --keyring-backend test --label "name service" $TXFLAG -y --no-admin
```

## Interaksi kontrak

Sekarang setelah kita menginstansikannya, kita bisa berinteraksi lebih lanjut dengan kontrak:

```sh
wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json
CONTRACT=$(wasmd query wasm list-contract-by-code $CODE_ID $NODE --output json | jq -r '.contracts[-1]')
echo $CONTRACT

wasmd query wasm contract $CONTRACT $NODE
wasmd query bank balances $CONTRACT $NODE
```

Hal ini memungkinkan kita untuk melihat alamat kontrak, rincian kontrak, dan saldo bank.

Sekarang, mari kita daftarkan nama ke kontrak untuk alamat dompet kita:

```sh
REGISTER='{"register":{"name":"fred"}}'
wasmd tx wasm execute $CONTRACT "$REGISTER" --amount 100uwasm --from $KEY_NAME $TXFLAG -y

# Query the owner of the name record
NAME_QUERY='{"resolve_record": {"name": "fred"}}'
wasmd query wasm contract-state smart $CONTRACT "$NAME_QUERY" $NODE --output json
```

Dengan itu, kita telah menginstansiasi dan berinteraksi dengan nameservice CosmWasm menggunakan Celestia!
