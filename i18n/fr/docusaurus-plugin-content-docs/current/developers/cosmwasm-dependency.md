---
sidebar_label: Les Dépendances CosmWasm
---

# Installations d'une Dépendance CosmWasm

## Configuration de l'Environnement

Pour ce tutoriel, nous allons utiliser `curl` and `jq`, des outils utiles.

Vous pouvez suivre le guide d'installation de ces outils [ici](./environment.md#setting-up-dependencies).

## Dépendance de Golang

La version de Golang utilisée pour ce tutoriel est la v1.18+

Si vous utilisez un système Linux, vous pouvez installer Golang en suivant notre tutoriel [ici](./environment.md#install-golang).

## Installation de Rust

### Rustup

Avant d'installer Rust, vous aurez besoin d'installer `rustup`.

Sur les systèmes Mac/Linux, les commandes pour l'installer sont :

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Après l'installation, suivez les commandes ci-dessous pour configurer Rust.

```sh
rustup default stable
cargo version

rustup target list --installed
rustup target add wasm32-unknown-unknown
```

## Installation de Docker

Nous utiliserons Docker plus tard dans ce tutoriel pour compiler un contrat intelligent pour utiliser une petite empreinte.

Nous recommandons donc d'installer Docker sur votre ordinateur.

Des examples d'installation de Docker sur Linux sont consultables [ici](https://docs.docker.com/engine/install/ubuntu/).

Trouvez les instructions spécifiques à votre système d'exploitation.

## Installation de wasmd

Ici, nous allons dérouler le dépôt `wasmd` et remplacer Tendermint par Rollmint. Rollmint est un le remplaçant idéal de Tendermint qui permet aux applications du Cosmos SDK de connecter le réseau d'accessibilité des données de Celestia.

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
