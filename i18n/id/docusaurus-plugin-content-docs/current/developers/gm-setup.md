---
sidebar_label: Atur
---

# 💻 Atur

- Sistem operasi: GNU/Linux, macOS, atau Windows Subsystem untuk Linux (WSL). Rekomendasi GNU/Linux atau macOS.

> Tata cara ini dibuat pada M2 Mac dengan macOS Monterey Versi 12.6.

- [Golang v1.18.2](https://go.dev/)
- [Ignite CLI v0.24.0](https://github.com/ignite/cli/releases/tag/v0.24.0)
- [Homebrew](https://brew.sh/)
- [wget](https://www.gnu.org/software/wget/)
- [jq](https://stedolan.github.io/jq/)
- [Node Light Celestia](https://docs.celestia.org/nodes/light-node/)

## 🏃 Instalasi Golang

Aplikasi-Celestia, Node-Celestia, dan Cosmos-SDK ditulis dengan bahasa pemrograman Golang. Kita butuh Golang untuk membuat dan menjalankannya. Testnet Mamaki Celestia membutuhkan Golang v1.18.2 untuk membuat dan menjalankannya dengan benar.

```bash
cd
ver="1.18.2"
wget "https://golang.org/dl/go$ver.darwin-arm64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.darwin-arm64.tar.gz"
rm "go$ver.darwin-arm64.tar.gz"
```

Add `/usr/local/go/bin` directory to [set your $PATH variables correctly](https://go.dev/doc/gopath_code#GOPATH):

```bash
# Jika menggunakan bash
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile

# Jika menggunakan zsh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.zshrc
source $HOME/.zshrc
```

Untuk memeriksa Go yang terinstall berjalan dengan baik:

```bash
go version
```

Hasil yang keluar harusnya versi terinstall:

```bash
go version go1.18.2 darwin/arm64
```

## 🔥 Instalasi Ignite CLI

Pertama, kamu butuh untuk membuat `/usr/local/bin` jika kamu tidak memilikinya:

```bash
sudo mkdir -p -m 775 /usr/local/bin
```

Jalankan perintah ini di terminalmu untuk instalasi Ignite CLI:

```bash
curl https://get.ignite.com/cli! | bash
```

> ✋ Dibeberapa mesin, kamu hanya bisa menjalankan sampai kesalahan perizinan seperti salah satu dibawah ini. Kamu dapat memperbaiki kesalahan ini dengan mengikuti panduan [disini](https://docs.ignite.com/guide/install#write-permission) dan dibawah ini.

```bash
# Error
jcs @ ~ % curl https://get.ignite.com/cli! | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3967    0  3967    0     0  38631      0 --:--:-- --:--:-- --:--:-- 41322
Installing ignite v0.24.0.....
######################################################################## 100.0%
**mv: rename ./ignite to /usr/local/bin/ignite: Permission denied**
============
**Error: mv failed**
jcs @ ~ %
```

Berikut perintah yang harusnya memperbaiki kesalahan perizinan:

```bash
sudo curl https://get.ignite.com/cli! | sudo bash
```

Instalasi yang berhasil akan kembali dengan respon yang sama seperti dibawah ini:

```bash
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3967    0  3967    0     0   4122      0 --:--:-- --:--:-- --:--:--  4136
Installing ignite v0.22.2.....
######################################################################## 100.0%
Installed at /usr/local/bin/ignite
```

Verifikasi Iginite CLI yang telah kamu install dengan menjalankan:

```bash
ignite version
```

Kamu akan mendapat respon seperti ini:

<!-- markdownlint-disable MD010 -->
<!-- markdownlint-disable MD013 -->
```bash
jcs @ ~ % ignite version
Ignite CLI version: v0.24.0
Ignite CLI build date:  2022-09-12T14:14:32Z
Ignite CLI source hash: 21c6430cfcc17c69885524990c448d4a3f56461c
Your OS:        darwin
Your arch:      arm64
Your go version:    go version go1.18.2 darwin/arm64
Your uname -a:      Darwin Joshs-MacBook-Air.local 21.6.0 Darwin Kernel Version 21.6.0: Sat Jun 18 17:07:28 PDT 2022; root:xnu-8020.140.41~1/RELEASE_ARM64_T8110 arm64
Your cwd:       /Users/joshcs
Is on Gitpod:       false
```
<!-- markdownlint-enable MD013 -->
<!-- markdownlint-enable MD010 -->

## 🍺 Instalasi Homebrew

Homebrew akan mengizinkan kita install depedensi untuk Mac kita:

```jsx
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Pastikan jalankan perintah agar hasil keluar sama seperti instalasi yang berhasil dibawah:

```jsx
==> Next steps:
- Run these three commands in your terminal to add Homebrew to your PATH:
    echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/joshstein/.zprofile
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/joshstein/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
```

## 🏃 Instalasi wget dan jq

wget ialah pemburu file Internet dan jq ialah otak baris perintah JSON yang ringan dan fleksibel.

```bash
brew install wget && brew install jq
```
