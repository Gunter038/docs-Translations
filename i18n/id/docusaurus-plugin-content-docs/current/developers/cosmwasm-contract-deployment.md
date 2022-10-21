---
sidebar_label: Peluncuran Kontrak
---

# Peluncuran kontrak pada CosmWasm dengan Rollmint
<!-- markdownlint-disable MD013 -->

## Penyatuan Smart Contract

Kita akan menjalankan perintah berikut untuk menurunkan smart contract Nameservice dan menyatukannya:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Kontrak yang menyatu keluar menjadi: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Tes Unit

Jika kita ingin menjalankan tes, kita dapat melakukannya dengan perintah berikut:

```sh
cargo unit-test
```

## Optimalisasi Smart Contract

Karena kita meluncurkan smart contract yang menyatu ke `wasmd`, kita ingin menjadikannya sekecil mungkin.

Tim CosmWasm menyediakan sebuah alat yang disebut `rust-optimizer` yang mana kita membutuhkan Docker untuk menyatukannya.

Jalankan perintah berikut:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Ini akan menjadi tempat optimalisasi bytecode Wasm pada `artifacts/cw_nameservice.wasm`.

## Peluncuran Kontrak

Ayo sekarang luncurkan smart contract kita!

Jalankan berikut ini:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Ini kamu akan mendapatkan hash transaksi dari peluncuran smart contract. Memberikan kita menggunakan Rollmint, disana akan ada hambatan pada transaksi termasuk yang dikarenakan antrian Rollmint pada Layer Ketersediaan Data Celestia untuk mengkonfirmasi block yang telah sebelumnya dikirim ke block baru.
