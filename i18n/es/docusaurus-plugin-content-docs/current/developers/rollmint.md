# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) is an ABCI (Application Blockchain Interface) implementation for sovereign rollups to deploy on top of Celestia.

Se construye reemplazando Tendermint, la capa de consenso Cosmos-SDK, con un reemplazo desplegable que se comunica directamente con la capa de disponibilidad de datos de Celestia.

Crea un rollup soberano, que recoge transacciones en bloques y las publica en Celestia para obtener consenso y disponibilidad de datos.

The goal of Rollmint is to enable anyone to design and deploy a sovereign rollup on Celestia in minutes.

Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Tutoriales

The following tutorials will help you get started building Cosmos-SDK applications that connect to Celestia's Data Availability Layer via Rollmint. Nosotros llamamos a esas cadenas Rollups Soberanas.

Puedes empezar con los siguientes tutoriales:

- [Wordle Game](./wordle.md)
- [Tutorial CosmWasm](./cosmwasm.md)
