# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint ](https://github.com/celestiaorg/rollmint) es una implementación ABCI (Aplicación de Interface Blockchain) para rollups soberanos a desplegar sobre Celestia.

Se construye reemplazando Tendermint, la capa de consenso Cosmos-SDK, como un sustituto inmediato que se comunica directamente con la capa de disponibilidad de datos de Celestia.

Crea un rollup soberano, que recoge transacciones en bloques y las publica en Celestia para obtener consenso y disponibilidad de datos.

El objetivo de Rollmint es permitir a cualquiera diseñar y desplegar un rollup soberano en Celestia en minutos.

Además, mientras que Rollmint te permite crear rollups soberanos en Celestia, actualmente no soporta pruebas de fraude aún y está por lo tanto funcionando en modo "pesimista", donde los nodos necesitarían volver a ejecutar las transacciones para comprobar la validez de la cadena (p.e. un nodo completo). Además, Optimint actualmente soporta un solo secuenciador.

## Tutoriales

Los siguientes tutoriales te ayudarán a comenzar a construir aplicaciones de Cosmos-SDK que se conectan a la disponibilidad de datos de Celestia Layer a través de Rollmint. Nosotros llamamos a esas cadenas Rollups Soberanas.

Puedes empezar con los siguientes tutoriales:

- [hola mundo](./gm-world.md)
- [Wordle Game](./wordle.md)
- [Tutorial CosmWasm](./cosmwasm.md)
