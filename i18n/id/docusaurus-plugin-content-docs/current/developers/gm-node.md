---
sidebar_label: Jalankan Light Node
---

# 🪶Menjalankan Celestia DA Light Node

Sebuah Celestia Light Node di Mamaki Testnet diperlukan untuk menyelesaikan tutorial ini. Jalankan perintah berikut untuk menginstal Celestia-Node:

<!-- markdownlint-disable MD010 -->
```bash
cd && rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node
git checkout tags/v0.3.0-rc2
make install
```
<!-- markdownlint-enable MD010 -->

![1.png](/img/gm/1.png)

Di dalam repositori celestia-node adalah utilitas bernama `cel-key` yang menggunakan utilitas yang menggunakan utilitas kunci yang disediakan oleh Cosmos-SDK di bawah tenda. Utilitas ini bisa digunakan untuk `add`, `delete`, dan mengelola kunci untuk setiap node DA jenis `(bridge || full || light)`, atau hanya kunci secara umum.

## 🗝 Membuat kunci

Buat kunci Anda untuk Node:

```bash
make cel-key
```

Verifikasi versi Celestia-Node Anda dengan perintah `celestia version` perintah, seharusnya `v0.3.0-rc2`:

```bash
celestia version
```

```bash
# OUTPUT

#Semantic version: v0.3.0-rc2
#Commit: 89892d8b96660e334741987d84546c36f0996fbe
#Build Date: Fri Oct  7 01:08:14 UTC 2022
#System version: amd64/linux
#Golang version: go1.18.2
```

## 🟢 Inisialisasi Light Node

Sekarang, kita siap untuk menginisialisasi Celestia Light Node. Anda bisa melakukannya dengan berlari:

```bash
celestia light init
```

Query alamat kunci kita menggunakan `cel-key` :

```bash
./cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 🚰 Kunjungi Faucet

Gunakan saluran `#mamaki-faucet` di Celestia Discord untuk meminta testnet token:

```bash
$request <Wallet-Address>
```

Mulai Node Celestia Light dengan koneksi ke Core Endpoint publik:

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![3.png](/img/gm/3.png)

Di jendela terminal lain, periksa saldo dari kunjungan kita ke keran:

```bash
curl -X GET http://localhost:26658/balance
```

Tanggapan Anda akan terlihat seperti ini, dalam format `utia` dalam format JSON.

```bash
{"denom":"utia","amount":"100000000"}
```

Sekarang kita sudah siap dengan Go dan Ignite CLI terinstal, dan Celestia Light Node kami berjalan di mesin kami, kami siap untuk membangun, menguji, dan meluncurkan kita sendiri.
