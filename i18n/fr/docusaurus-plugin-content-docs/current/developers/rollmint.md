# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

Il est construit en remplacement de Tendermint, la couche de consensus du Cosmos-SDK, avec une solution prête à l'usage qui communique directement avec la couche d'Accessibilité des Données de Celestia.

Il fait tourner un rollup souverain, qui collecte les transactions dans les blocs et les publie sur Celestia pour le consensus et l'accessibilité des données.

The goal of Rollmint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Tutoriels

The following tutorials will help you get started building Cosmos-SDK applications that connect to Celestia's Data Availability Layer via Rollmint. Nous appelons ces chaines les Rollups Souverains.

Vous pouvez commencer avec les tutoriels suivants :

- [Jeu du Wordle](./wordle.md)
- [Tutoriel CosmWasm](./cosmwasm.md)
