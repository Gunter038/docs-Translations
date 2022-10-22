---
sidebar_label: Ketergantungan CosmWasm
---

# Instalasi Ketergantungan CosmWasm

## Penyiapan Lingkungan

For this tutorial, we will be using `curl` and `jq` as helpful tools.

Anda dapat mengikuti panduan untuk menginstalnya [di sini](./environment.md#setting-up-dependencies).

## Ketergantungan Golang

Versi Golang yang digunakan untuk tutorial ini adalah v1.18+

If you are using a Linux distribution, you can install Golang by following our tutorial [here](./environment.md#install-golang).

## Pemasangan Karat

### Rustup

Pertama, sebelum menginstall Rust, anda harus menginstall `rustup`.

Pada sistem Mac/Linux, berikut ini adalah perintah untuk menginstalnya:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Setelah instalasi, ikuti perintah di sini untuk menyiapkan Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Instalasi Docker

Kita akan menggunakan Docker nanti dalam tutorial ini untuk menyusun smart contract untuk menggunakan footprint yang kecil.

Kami merekomendasikan untuk menginstal Docker pada mesin Anda.

Contoh-contoh tentang cara menginstalnya di Linux dapat ditemukan [di sini](https://docs.docker.com/engine/install/ubuntu/).

Temukan instruksi yang tepat khusus untuk OS Anda.

## instalasi wasmd

Di sini, kita akan menarik repositori `wasmd` dan mengganti Tendermint dengan Rollmint. Rollmint adalah pengganti drop-in untuk Tendermint yang memungkinkan Cosmos-SDK untuk terhubung ke jaringan Ketersediaan Data Celestia.

```sh
git clone https://github.com/CosmWasm/wasmd.git
cd wasmd
git fetch --tags
git checkout v0.27.0
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy 
go mod download
make install
```
