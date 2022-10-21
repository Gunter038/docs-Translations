---
sidebar_label: Зависимости CosmWasm
---

# Установка зависимостей CosmWasm

## Установка среды

Для этого руководства мы будем использовать `curl` и `jq` в качестве полезных инструментов.

Вы можете следовать руководству по их установке [тут](./environment.md#setting-up-dependencies).

## Зависимось Golang

Версия Golang, используемая в этом руководстве, 1.18+

Если вы используете операционную систему Linux, вы можете установить Golang следуя нашему руководству [здесь](./environment.md#install-golang).

## Установка Rust

### Rustup

Прежде чем установить Rust, вам нужно установить `rustup`.

На системах Mac/Linux есть команды для его установки:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

После завершения установки выполните приведенные здесь команды для запуска Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Установка Docker

Позже в этом руководстве мы будем использовать Docker для компиляции смарт-контракта, чтобы он занимал мало места.

Мы рекомендуем установить Docker на вашу компьютерную систему.

Примеры того, как установить его в Linux, можно найти [тут](https://docs.docker.com/engine/install/ubuntu/).

Найдите правильные инструкции, подходящие именно для вашей ОС.

## Установка wasmd

Здесь мы собираемся взять репозиторий `wasmd` и заменить Tendermint на Rollmint. Rollmint - это замена Tendermint, которая позволяет приложениям Cosmos-SDK подключаться к сети Celestia's Data Availability.

```sh
git clone https://github.com/CosmWasm/wasmd.git
cd wasmd
git fetch --tags
git checkout v0.27.0
go mod edit -replace github. om/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
перейти в мод 
перейти к моду загрузить
make install
```
