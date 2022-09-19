---
sidebar_label: Dependencias CosmWasm
---

# Instalaciones de dependencias de CosmWasm

## Configuracion de entorno

Para este tutorial, usaremos `curl` y `jq` como herramientas útiles.

Puedes seguir la guía sobre cómo instalarlos [aquí](./environment.md#setting-up-dependencies).

## Dependencia de Golang

La versión de Golang utilizada para este tutorial es v1.18+

Si estás usando una distribución Linux, puedes instalar Golang siguiendo nuestro tutorial [aquí](./environment.md#install-golang).

## Instalación de Rust

### Rustup

Primero, antes de instalar Rust, necesitarás instalar `rustup`.

En sistemas Mac/Linux, aquí están los comandos para instalarlos:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Después de la instalación, sigue los comandos aquí para configurar Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Guía de instalación de Docker

Utilizaremos Docker más adelante en este tutorial para compilar un smart contract para usar una pequeña huella.

Te recomendamos instalar Docker en tu máquina.

Los ejemplos sobre cómo instalarlo en Linux se encuentran [aquí](https://docs.docker.com/engine/install/ubuntu/).

Encuentra las instrucciones adecuadas específicas para tu sistema operativo.

## instalación de wasmd

Aquí vamos a descargar el repositorio `wasmd` y reemplazar Tendermint con Optimint. Optimint es un reemplazo para Tendermint que permite a aplicaciones Cosmos-SDK conectarse a la red de Disponibilidad de Datos de Celestia.

```sh
git clone https://github.com/CosmWasm/wasmd.git
cd wasmd
git fetch --tags
git checkout v0.27.0
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy 
go mod download
make install
```
