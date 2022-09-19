# Optimint

![optimint](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

Його створено шляхом заміни Tendermint, консенсусного рівня Cosmos-SDK, заміна якого безпосередньо взаємодіє з рівнем доступності даних Celestia.

It spins up a sovereign rollup, which collects transactions into blocks and posts them onto Celestia for consensus and data availability.

The goal of Optimint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Optimint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Optimint currently only supports a single sequencer.

## Навчальні посібники

Наступні навчальні посібники допоможуть вам розпочати створення програм Cosmos-SDK, які підключаються до рівня доступності даних Celestia через Optimint. Ми називаємо ці ланцюги Sovereign Rollups.

Ви можете почати з наступних підручників:

- [Гра Wordle](./wordle.md)
- [Посібник CosmWasm](./cosmwasm.md)
