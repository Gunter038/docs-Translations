---
sidebar_label: Ikhtisar CosmWasm
---

# CosmWasm pada Rollmint

CosmWasm adalah platform kontrak pintar yang dibangun untuk ekosistem Cosmos dengan memanfaatkan WebAssembly (Wasm) untuk membangun kontrak pintar untuk Cosmos-SDK. untuk Cosmos-SDK. Dalam tutorial ini, kita akan mengeksplorasi cara mengintegrasikan CosmWasm dengan Lapisan Ketersediaan Data Celestia menggunakan Rollmint.

> CATATAN: Tutorial ini akan mengeksplorasi pengembangan dengan Rollmint, yang masih dalam tahap Alpha. Jika Anda mengalami bug, silakan tulis tiket Masalah Github atau beri tahu kami di Discord kami. Selain itu, sementara Rollmint memungkinkan Anda untuk membangun rollup berdaulat di Celestia, saat ini belum mendukung bukti penipuan dan oleh karena itu oleh karena itu berjalan dalam mode "pesimis", di mana node perlu jalankan kembali transaksi untuk memeriksa validitas rantai (yaitu simpul penuh). Lebih jauh lagi, Rollmint saat ini hanya mendukung sequencer tunggal.

Anda dapat mempelajari lebih lanjut tentang CosmWasm [di sini](https://docs.cosmwasm.com/docs/1.0/).

Dalam tutorial ini, kita akan membahas hal-hal berikut ini:

* [Menyiapkan dependensi Anda untuk kontrak pintar CosmWasm Anda](./cosmwasm-dependency.md)
* [Menyiapkan Rollmint pada CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instantiate jaringan lokal untuk rantai CosmWasm Anda yang terhubung ke Celestia](./cosmwasm-environment.md)
* [Menyebarkan kontrak pintar Rust ke rantai CosmWasm](./cosmwasm-contract-deployment.md)
* [Berinteraksi dengan kontrak pintar](./cosmwasm-contract-interaction.md)

Smart contract yang akan kita gunakan untuk tutorial ini adalah yang disediakan oleh tim CosmWasm untuk pembelian Nameservice.

Anda bisa melihat kontrak [di sini](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Bagaimana menulis kontrak pintar Rust untuk Nameservice berada di luar cakupan tutorial ini. Di masa mendatang kami akan menambahkan lebih banyak tutorial untuk menulis CosmWasm kontrak pintar untuk Celestia.
