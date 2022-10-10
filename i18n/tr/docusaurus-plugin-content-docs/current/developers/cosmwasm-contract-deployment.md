---
sidebar_label: Optimint ile CosmWasm üzerinde Sözleşme Dağıtımı
---

# Contract Deployment on CosmWasm with Rollmint
<!-- markdownlint-disable MD013 -->

## Akıllı Sözleşmeyi Derleme

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

CosmWasm ekibi, derlemek için Docker'a ihtiyaç duyduğumuz `rust-optimizer` adlı bir araç sağlıyor.

Bu komutu çalıştırın:

```sh
sudo docker run --rm -v "$(pwd)":/code \
  --mount type=volume,source="$(basename "$(pwd)")_cache",target=/code/target \
  --mount type=volume,source=registry_cache,target=/usr/local/cargo/registry \
  cosmwasm/rust-optimizer:0.12.6
```

Bu, optimize edilmiş Wasm bayt kodunu `artifacts/cw_nameservice.wasm` 'a yerleştirecektir.

## Sözleşme Dağıtımı

Şimdi akıllı sözleşmemizi dağıtalım!

Bu komutu çalıştırın:

```sh
TX_HASH=$(wasmd tx wasm store artifacts/cw_nameservice.wasm --from $KEY_NAME --keyring-backend test $TXFLAG --output json -y | jq -r '.txhash') 
```

Bu size akıllı sözleşme dağıtımı için işlem hash'ini sağlayacaktır. Given we are using Rollmint, there will be a delay on the transaction being included due to Rollmint waiting on Celestia's Data Availability Layer to confirm the block has been included before submitting a new block.
