---
sidebar_label: Contract Deployment
---

# Rollmint ile CosmWasm'da Sözleşme Dağıtımı
<!-- markdownlint-disable MD013 -->

## Compile the Smart Contract

Nameservice akıllı sözleşmesini indirip derlemek için aşağıdaki komutları çalıştıracağız:

```sh
git clone https://github.com/InterWasm/cw-contracts
cd cw-contracts
cd contracts/nameservice
cargo wasm
```

Derlenmiş sözleşme şu adrese çıkarılır: `target/wasm32-unknown-unknown/release/cw_nameservice.wasm`.

## Birim testleri

Test etmek istiyorsak, aşağıdaki komutla bunu yapabiliriz:

```sh
cargo unit-test
```

## Optimize Edilmiş Akıllı Sözleşme

Derlenmiş akıllı sözleşmeyi `wasmd` 'a yerleştirdiğimiz için, mümkün olduğunca küçük olmasını istiyoruz.

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
