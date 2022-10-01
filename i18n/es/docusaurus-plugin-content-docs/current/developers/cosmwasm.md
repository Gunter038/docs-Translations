---
sidebar_label: Vista general de CosmWasm
---

# CosmWasm en Optimint

CosmWasm es una plataforma de smart contracts construida para el ecosistema de Cosmos haciendo uso de WebAssembly (Wasm) para construir smart contracts para Cosmos-SDK. En este tutorial, vamos a explorar cómo integrar CosmWasm con la capa de disponibilidad de datos de Celestia usando Optimint.

> NOTA: Este tutorial explorará el desarrollo con Optimint, que todavía está en fase Alfa. Si encuentras errores, por favor escribe un ticket de Github Issue o háznoslo saber en nuestro Discord. Además, mientras que Optimint te permite crear registros soberanos en Celestia, actualmente no soporta pruebas de fraude aún y está por lo tanto funcionando en modo "pesimista", donde los nodos necesitarían volver a ejecutar las transacciones para comprobar la validez de la cadena (i.. un nodo completo). Además, Optimint actualmente sólo soporta un solo secuenciador.

Puedes aprender más sobre CosmWasm [aquí](https://docs.cosmwasm.com/docs/1.0/).

En este tutorial, repasaremos lo siguiente:

* [Configurando tus dependencias para tus smart contracts de CosmWasm](./cosmwasm-dependency.md)
* [Configurando Optimint en CosmWasm](./cosmwasm-dependency.md#wasmd-installation)
* [Instanciar una red local para tu cadena CosmWasm conectada a Celestia](./cosmwasm-environment.md)
* [Implementando un smart contract de Rust a la cadena CosmWasm](./cosmwasm-contract-deployment.md)
* [Interactuando con el smart contract](./cosmwasm-contract-interaction.md)

El smart contract que usaremos para este tutorial es uno proporcionado por el equipo de CosmWasm para la compra de Nameservice.

Puedes revisar el contrato [aquí](https://github.com/InterWasm/cw-contracts/tree/main/contracts/nameservice).

Cómo escribir el smart contract de Rust para Nameservice está fuera del alcance de este tutorial. En el futuro añadiremos más tutoriales para escribir smart contracts de CosmWasm para Celestia.
