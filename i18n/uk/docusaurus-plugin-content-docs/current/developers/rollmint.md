# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

Його створено шляхом заміни Tendermint, консенсусного рівня Cosmos-SDK, заміна якого безпосередньо взаємодіє з рівнем доступності даних Celestia.

Він запускає суверенний ролап, який збирає транзакції в блоки та публікує їх у Celestia для консенсусу та доступності даних.

The goal of Rollmint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Навчальні посібники

The following tutorials will help you get started building Cosmos-SDK applications that connect to Celestia's Data Availability Layer via Rollmint. Ми називаємо ці ланцюги Sovereign Rollups.

Ви можете почати з наступних підручників:

- [Гра Wordle](./wordle.md)
- [Посібник CosmWasm](./cosmwasm.md)
