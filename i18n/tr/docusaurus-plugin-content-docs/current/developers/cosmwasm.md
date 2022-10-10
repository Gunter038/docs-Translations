---
sidebar_label: Optimint üzerinde CosmWasm
---

# CosmWasm on Optimint

CosmWasm, Cosmos-SDK için akıllı sözleşmeler oluşturmak için WebAssembly'den (Wasm) yararlanarak Cosmos ekosistemi için oluşturulmuş bir akıllı sözleşme platformudur. Bu rehberde, CosmWasm'ı Celestia'nın Veri Kullanılabilirlik Katmanı ile Optimint kullanarak nasıl entegre edeceğimizi keşfedeceğiz.

> NOT: Bu rehber, hala Alfa evresinde olan Optimint ile geliştirmeyi keşfedecektir. Hatalar ile karşılaşırsanız, lütfen bir Github issue ticketi açın veya Discord üzerinden bize bildirin. Ayrıca, Optimint Celestia üzerinde bağımsız rollup'lar oluşturmanıza izin verirken,  henüz dolandırıcılık kanıtlarını (fraud proofs) desteklememektedir ve  bu yüzden düğümlerin zincirin geçerliliğini kontrol etmek için işlemleri yeniden yürütmesi gerekeceği "pesimist" modda çalışmaktadır. (ör. full node)). Buna ek olarak, Optimint şu anda yalnızca tek bir sıralayıcıyı desteklemektedir.

You can learn more about CosmWasm [here](https://docs.cosmwasm.com/docs/1.0/).

In this tutorial, we will going over the following:

* [Setting up your dependencies for your CosmWasm smart contracts](./cosmwasm-dependency.md)
* [Setting up Optimint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instantiate a local network for your CosmWasm chain connected to Celestia](./cosmwasm-environment.md)
* [Deploying a Rust smart contract to CosmWasm chain](./cosmwasm-contract-deployment.md)
* [Interacting with the smart contract](./cosmwasm-contract-interaction.md)

The smart contract we will use for this tutorial is one provided by the CosmWasm team for Nameservice purchasing.

You can check out the contract [here](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

How to write the Rust smart contract for Nameservice is outside the scope of this tutorial. In the future we will add more tutorials for writing CosmWasm smart contracts for Celestia.
