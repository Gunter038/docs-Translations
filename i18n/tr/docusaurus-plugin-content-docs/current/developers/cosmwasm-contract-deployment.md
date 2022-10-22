---
sidebar_label: Contract Deployment
---

# Rollmint ile CosmWasm'da Sözleşme Dağıtımı
<!-- markdownlint-disable MD013 -->

## Compile the Smart Contract

Nameservice akıllı sözleşmesini çekmek ve  bunu derlemek için aşağıdaki komutları çalıştıracağız: git clone https://github.com/InterWasm/cw-contracts cd cw-contracts cd contracts/nameservice cargo wasm

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

The compiled contract is outputted to: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Unit Tests

Testleri çalıştırmak istiyorsak eğer, aşağıdaki komutla bunu yapabiliriz: cargo unit-test

```sh
cargo unit-test
```

## Optimized Smart Contract

Derlenmiş akıllı sözleşmeyi `wasmd` 'a yerleştirdiğimiz için, mümkün olduğunca bunun küçük olmasını istiyoruz.

CosmWasm team provides a tool called `rust-optimizer` which we need Docker for in order to compile.

Run the following command:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

This will place the optimized Wasm bytecode at `artifacts/cw_nameservice.wasm`.

## Contract Deployment

Let's now deploy our smart contract!

Run the following:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Bu size akıllı sözleşme dağıtımı için işlem hash'ini sağlayacaktır. Rollmint kullandığımızdan,Rolmint'in yeni bir blok göndermeden önce bloğu onaylamak için Celestia'nın Veri Kullanılabirlik Katmanında bekleme olması nedeniyle dahil edilen işlemlerde bir geçikme olacaktır.
