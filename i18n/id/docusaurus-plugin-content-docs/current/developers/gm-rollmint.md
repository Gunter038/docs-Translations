---
sidebar_label: Bangun Sovereign Rollup
---

# 🗞 Membangun Sovereign Rollup

Ignite CLI datang dengan peerintah scaffolding untuk membuat pengembangan dari blockchain lebih cepat dengan membuat semua yang dibutuhkan untuk memulai sebuah blockchain Cosmos SDK baru.

Buka tab baru atau windows di terminalmu dan jalankan perintah ini ke scaffold rollupmu:

```bash
ignite scaffold chain gm
```

Respon akan seperti dibawah ini:

```bash
jcs @ ~ % ignite scaffold chain gm

⭐️ Successfully created a new blockchain 'gm'.
👉 Get started with the following commands:

 % cd gm
 % ignite chain serve

Documentation: https://docs.ignite.com
```

Perintah ini telah membuat blockchain Cosmos SDK di direktori `gm`. Direktori `gm` mengandung blockchain yang sepenuhnya berfungsi. Berikut standar [modul](https://docs.cosmos.network/master/modules/) Cosmos SDK yang telah diimpor:

- `staking` - untuk delegasi konsensus mekanisme Proof-of-Stake (PoS)
- `bank` - untuk transfer token yang berfungsi antara akun
- `gov` - untuk governance di chain
- `mint` - untuk mencetak unit baru dari token staking
- `nft` - untuk membuat, mentransfer, dan memperbarui NFT
- dan [lebih banyak lagi](https://docs.cosmos.network/master/architecture/adr-043-nft-module.html)

Ubah direktori `gm`:

```bash
cd gm
```

Kamu dapat belajar lebih lanjut tentang stuktur file direktori `gm` [disini](https://docs.ignite.com/guide/hello#blockchain-directory-structure). Sebagian besar kami kerjakan dalam tata cara ini yang akan muncul di direktori `x`.

## 💎 Instalasi Rollmint

Untuk berpindah Tendermint ke Rollmint, Jalankan perintah berikut:

```bash
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy
go mod download
```

## 🎬 Memulai blockchain

Sekarang kita memiliki scaffolded rollup yang berfunsi sepenuhnya, kita dapat memulai chain kita pada mesin kita dengan menjalankan perintah ini dalam direktori `gm`:

```bash
ignite chain serve
```

Respon di terminalmu akan seperti dibawah ini:

```bash
Cosmos SDK's version is: stargate - v0.46.1

🛠️  Building proto...
📦 Installing dependencies...
🛠️  Building the blockchain...
💿 Initializing the app...
🙂 Created account "alice" with address
"cosmos1x68wdvng7w56h48t7tmnccfg84uxe76yppjc6j"
with mnemonic: "breezegarage boil under old useless vessel shoulder donkey
deputy ripple mention air remain find tent bright ill judge effort small lazy
salmon oppose"
🙂 Created account "bob" with address
"cosmos1uzwtd2lts0ak7dhha8d4gaqsd7ucph90gqdxrw"
with mnemonic: "excuse frozen level baby virus beauty pitch pill lobster argue
teach half loan argue wing daughter kit episode diary exhibit material fortune
learn wool"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Perintah `ignite chain serve` menggunduh dependensi dan menyatukan source code kedalam binary yang disebut `gmd` (repo + `d`). Mulai sekarang, kamu akan menggunakan `gmd` untuk menjalankan semua perintah chainmu.

### 🛑 Berhentikan blockchainmu

Untuk memberhentikan blockchainmu, tekan `Ctrl + C` di window terminal yang mana sedang berjalan. Kita siap untuk mempersiapkan query Sovereign Rollup pertama kita dan menghubungkan ke layer Ketersediaan Data Celestia (DA).
