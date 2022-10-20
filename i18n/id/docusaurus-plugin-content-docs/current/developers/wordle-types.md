---
sidebar_label: Tipe
---

# Jenis Wordle

Untuk langkah selanjutnya, kita akan membuat tipe yang akan digunakan oleh pesan yang kita buat.

## Jenis Wordle Perancah

```sh
ignite scaffold map wordle word submitter --no-message
```

Jenis ini adalah peta yang disebut `Wordle` dengan dua nilai dari `word` dan `submitter`. `submitter` adalah alamat dari orang yang mengirimkan Wordle.

Tipe kedua adalah tipe `Guess`. Ini memungkinkan kita untuk menyimpan tebakan terbaru untuk setiap alamat yang mengirimkan solusi.

```sh
ignite scaffold map guess word submitter count --no-message
```

Di sini, kita juga menyimpan `count` untuk menghitung berapa banyak tebakan alamat ini yang dikirimkan.
