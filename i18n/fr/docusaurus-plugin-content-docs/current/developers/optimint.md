# Optimint

![optimint](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

It is built by replacing Tendermint, the Cosmos-SDK consensus layer, with a drop-in replacement that communicates directly with Celestia's Data Availability layer.

It spins up a sovereign rollup, which collects transactions into blocks and posts them onto Celestia for consensus and data availability.

The goal of Optimint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Optimint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Optimint currently only supports a single sequencer.

## Tutorials

Les tutoriels suivants vont vous aider à débuter la construction d'applications Cosmos-SDK capables de se connecter à la couche d'accessibilité des données de Celestia, via Optimint. We call those chains Sovereign Rollups.

Vous pouvez commencer avec les tutoriels suivants :

- [Jeu du Wordle](./wordle.md)
- [Tutoriel CosmWasm](./cosmwasm.md)
