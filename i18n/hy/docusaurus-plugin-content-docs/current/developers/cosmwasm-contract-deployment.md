---
sidebar_label: Պայմանագրի տեղակայումը
---

# Պայմանագրի տեղակայում CosmWasm-ում Rollmint-ի հետ
<!-- markdownlint-disable MD013 -->

## Կազմել Սմարթ-Պայմանագիր

Մենք կկատարենք հետևյալ հրամանները՝ Nameservice-ի սմարթ-պայմանագիր հանելու և այն կազմելու համար:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Կազմված պայմանագիրը դուրս է բերվում դեպի `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Մոդուլային թեստ

Եթե մենք ուզում ենք թեստեր թողարկել, մենք կարող ենք դա անել հետեւյալ հրամանի միջոցով:

```sh
cargo unit-test
```

## Օպտիմիզացված սմարթ պայմանագիր

Քանի որ մենք հավաքում ենք սմարթ-պայմանագիր `wasmd`, մենք ուզում ենք, որ այն հնարավորինս փոքր լինի.

CosmWasm թիմը տրամադրում է գործիք, որը կոչվում է `rust-optimizer`, որի կազմման համար մեզ անհրաժեշտ է Docker.

Հետեւեք հետեւյալ հրամանին:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Արդյունքում, օպտիմիզացված բայթկոդ Wasm- ը հասանելի կլինի այստեղ `artifacts/cw_nameservice.wasm`.

## Պայմանագրի տեղակայումը

Եկեք հիմա տեղակայենք մեր սմարթ-պայմանագիրը!

Կատարեք հետեւյալը:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Սա ձեզ թույլ կտա խեշ-գործարք ձեռք բերել սմարթ-պայմանագիր կնքելու համար. Հաշվի առնելով, որ մենք օգտագործում ենք Rollmint- ը, գործարքը հետաձգվելու է այն պատճառով, որ Rollmint- ը ակնկալում է, որ Celestia- ի տվյալների առկայության շերտը հաստատմանը, որ նոր բլոկը ուղարկվել է.
