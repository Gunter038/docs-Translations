---
sidebar_label: Огляд CosmWasm
---

# CosmWasm на Optimint

CosmWasm — це смартконтрактна платформа, створена для екосистеми Cosmos шляхом використання WebAssembly (Wasm) для створення смартконтрактів для Cosmos-SDK. У цьому посібнику ми розглянемо, як інтегрувати CosmWasm із рівнем доступності даних Celestia за допомогою Optimint.

> ПРИМІТКА. У цьому підручнику розглядається розробка за допомогою Optimint, яка все ще перебуває на стадії альфа-версії. Якщо ви зіткнетеся з помилками, будь ласка, напишіть заявку на Github Issue або повідомте нам про це в нашому Discord. Крім того, хоча Optimint дозволяє вам створювати суверенні зведені пакети на Celestia, він наразі ще не підтримує докази шахрайства, тому працює в «песимістичному» режимі, де вузли мають повторно виконати транзакції, щоб перевірити дійсність ланцюжка (тобто повну ноду). Крім того, наразі Optimint підтримує лише один секвенсер.

Про CosmWasm можна дізнатися більше [тут](https://docs.cosmwasm.com/docs/1.0/).

У цьому посібнику ми переходимо до наступних кроків:

* [Setting up your dependencies for your CosmWasm smart contracts](./cosmwasm-dependency.md)
* [Setting up Optimint on CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instantiate a local network for your CosmWasm chain connected to Celestia](./cosmwasm-environment.md)
* [Deploying a Rust smart contract to CosmWasm chain](./cosmwasm-contract-deployment.md)
* [Interacting with the smart contract](./cosmwasm-contract-interaction.md)

The smart contract we will use for this tutorial is one provided by the CosmWasm team for Nameservice purchasing.

You can check out the contract [here](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

How to write the Rust smart contract for Nameservice is outside the scope of this tutorial. In the future we will add more tutorials for writing CosmWasm smart contracts for Celestia.
