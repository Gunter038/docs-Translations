# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) adalah ABCI (Aplikasi Antarmuka Blockchain) mengimplementasikan untuk sovereign rollups yang diluncurkan ke atas Celestia.

Ini dibangun dengan menganti Tendermint, layer konsensus Cosmos-SDK, dengan jatuhnya kedalam pengganti yang berkomunikasi secara langsung dengan layer Ketersediaan Data Celestia.

Sovereign rollup ini berputar, yang mana mengumpulkan transaksi ke block dan mengirimnya kedalam Celestia untuk konsensus dan ketersediaan data.

Tujuan Rollmint adalah mengizinkan siapapun untuk mendesain dan meluncurkan sovereign rollup pada Celestia dalam beberapa menit.

Selain itu, ketika Rollmint mengizinkanmu untuk membangun sovereign rollup pada Celestia, saat ini tidak mendukung fraud proof dan oleh karena itu berjalan dalam mode "pessimistic", dimana node butuh mengeksekusi ulang transaksi untuk memeriksa validasi dari chain (yaitu full node). Selanjutnya, Rollmint saat ini hanya mendukung sequencer tunggal.

## Tata cara

Mengikuti tata cara akan membantumu dalam memulai membangun aplikasi Cosmos-SDK yang terhubung ke Layer Ketersediaan Data Celestia melalui Rollmint. Kita menyebutnya chain Sovereign Rollups.

Kamu dapat memulai dengan mengikuti tata cara berikut:

- [gm world](./gm-world.md)
- [Permainan Wordle](./wordle.md)
- [Tata cara CosmWasm](./cosmwasm.md)
