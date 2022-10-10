---
sidebar_label: Залежності CosmWasm
---

# Встановлення залежностей CosmWasm

## Налаштування середовища

У цьому посібнику ми будемо використовувати `curl` і `jq` як корисні інструменти.

Ви можете дотримуватися інструкцій щодо їхнього встановлення, викладених [тут](./environment.md#setting-up-dependencies).

## Залежність Ґоланґа

У цьому підручнику використовується версія Golang v1.18+

Якщо ви використовуєте дистрибутив Linux, ви можете встановити Golang, дотримуючись нашого підручника [тут](./environment.md#install-golang).

## Встановлення Rust

### Rustup

Спочатку перед встановленням Rust, вам необхідно встановити `rustup`.

Ось команди для встановлення на системах Mac/Linux:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Після установки виконайте команди, які допоможуть налаштувати Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Встановлення Docker

Пізніше у цьому підручнику ми будемо використовувати Docker для компіляції смартконтракту, щоб використовувати невелику площу.

Ми рекомендуємо встановити Docker на ваш комп'ютер.

Приклади того, як встановити його на Linux, є [тут](https://docs.docker.com/engine/install/ubuntu/).

Знайдіть потрібні інструкції для вашої ОС.

## Встановлення wasmd

Here, we are going to pull down the `wasmd` repository and replace Tendermint with Rollmint. Rollmint is a drop-in replacement for Tendermint that allows Cosmos-SDK applications to connect to Celestia's Data Availability network.

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
