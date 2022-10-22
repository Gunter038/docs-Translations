---
sidebar_label: Menyiapkan Lingkungan Jaringan
---

# Menyiapkan Lingkungan Anda untuk CosmWasm di Celestia

Now the `wasmd` binary is built, we need to setup a local network that communicates between `wasmd` and Rollmint.

## Membangun Jaringan Wasmd

Jalankan perintah berikut ini:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Ini menginisialisasi rantai yang disebut `celeswasm` dengan `wasmd` binary.

Perintah berikut ini membantu kita menyiapkan akun untuk genesis:

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Pastikan Anda menyimpan output dari dompet yang dihasilkan untuk referensi di kemudian hari jika diperlukan.

Sekarang, mari kita tambahkan akun genesis dan menggunakannya untuk memperbarui file genesis kita:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Dengan itu, kami membuat file genesis jaringan lokal.

Beberapa perintah yang lebih berguna yang bisa kita siapkan:

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## Membangun Jaringan Wasmd

Kita dapat menjalankan yang berikut ini untuk memulai jaringan `wasmd`:

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Mohon pertimbangkan:

> CATATAN: Dalam perintah di atas, Anda harus memberikan alamat IP Celestia Node ke `base_url` yang memiliki akun dengan token Arabica Devnet. Follow tutorial untuk menyiapkan Celestia Light Node dan membuat dompet dengan uang keran testnet [here](./node-tutorial.md) di bagian Celestia Node.

Juga harap dipertimbangkan:

> PENTING: Selanjutnya, dalam perintah di atas, Anda harus menentukan Block Height terbaru di Arabica Devnet untuk `da_height`. Anda bisa menemukan nomor blok terbaru di penjelajah [disini](https://explorer.celestia.observer/arabica). Juga, untuk bendera `--rollmint.namespace_id`, Anda dapat menghasilkan Namespace acak ID acak menggunakan taman bermain [here](https://go.dev/play/p/7ltvaj8lhRl)

Dengan itu, kita telah memulai jaringan `wasmd` kita!
