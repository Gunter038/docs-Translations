---
sidebar_label: Ikhtisar Wordle
---

# Aplikasi Wordle pada Rollmint

![mamaki-testnet](/img/wordle.jpg)

Panduan tata cara ini akan menuju pembanggunan sebuah aplikasi cosmos-sdk untuk Rollmint, implementasi tendermint dari Sovereign-Rollup, untuk permainan terkenal [Wordle](https://www.nytimes.com/games/wordle/index.html).

Panduan ini akan menunju bagaimana cara mengatur Rollmint di Ignite CLI dan menggunakannya untuk membuat permainan. Panduan ini akan menuju desain sederhana, yang mana termasuk dengan implementasi masa depan dan ide untuk memperluas basis kode ini.

> CATATAN: panduan ini akan menjelajahi pengembangan dengan Rollmint, yang mana masih dalam tahap Alpha. Jika kamu menemukan bug, tolong tulis tiket masalah Github atau beritahu kami di Discord kami. Selain itu, ketika Rollmint mengizinkanmu untuk mambangun sovereign rollups di Celestia, saat ini belum mendukung fraud proofs dan karena itu dijalankan dalam mode "pessimistic", yang dimana node butuh untuk eksekusi ulang transaksi untuk memeriksa validasi dari chain (yaitu full node). Selanjutnya, Rollmint saat ini hanya mendukung sequencer tunggal.

## Prasyarat

Tata cara ini diberikan bertujuan untuk pengembang yang berpengalaman dalam Cosmos-SDK, kita merekomendasikanmu tata cara yang bisa kamu ikuti dalam Ignite agar semua komponen yang berbeda dapat dipahami pada Cosmos-SDK sebelum melakukan dengan tata cara ini.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Dasar Blog dan Modul](https://docs.ignite.com/guide/blog)
* [Tata cara Nameservice](https://docs.ignite.com/guide/nameservice)
* [Scavenger Hunt](https://docs.ignite.com/guide/scavenge)

Kamu tidak perlu melakukan panduan tersebut untuk mengikuti tata cara Wordle ini, tapi akan sangat membantu jika kamu memahami arsitektur Cosmos-SDK.

## Implementasi Desain

Aturan Wordle sederhana: Kamu hanya menebak kata hari ini.

Titik Kunci untuk dipertimbangkan:

* Kata berisi kata lima huruf.
* Kamu punya 6 kesempatan menebak.
* Setiap 24 jam, ada kata baru.

GUI pada Wordle menunjukkanmu beberapa indikator: sorotan hijau pada huruf diposisi tersebut bermakna huruf tersebut benar untuk posisi Wordle yang benar. Sorotan kuning bermakna huruf tersebut benar untuk Wordle dalam posisi yang salah. Sorotan abu-abu bermakna huruf bukan bagian dari Wordle.

For simplicity of the design, we will avoid those hints, although there are ways to extend this codebase to implement that, which we will show at the end.

In this current design, we implement the following rules:

* 1 Wordle can be submitted per day
* Every address will have 6 tries to guess the word
* It must be a five-letter word.
* Whoever guesses the word correctly before their 6 tries are over gets an award of 100 WORDLE tokens.

We will go over the architecture to achieve this further in the guide. But for now, we will get started setting up our development environment.

## Daftar Isi Tata Cara ini

Tata cara telah dipecah kedalam pilihan sebagai berikut:

1. [Ignite dan Chain Scaffolding](./scaffold-wordle.md)
2. [Instalasi Rollmint](./install-rollmint.md)
3. [Modul](./wordle-module.md)
4. [Pesan](./wordle-messages.md)
5. [Tipe](./wordle-types.md)
6. [Penjaga](./wordle-keeper.md)
7. [Menjalankan Wordle](./run-wordle.md)
