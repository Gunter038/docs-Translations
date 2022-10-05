# Optimint

![optimint](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) es una implementación ABCI Blockchain Interface) para rollups soberanos a desplegar sobre Celestia.

Se construye reemplazando Tendermint, la capa de consenso Cosmos-SDK, con un reemplazo desplegable que se comunica directamente con la capa de disponibilidad de datos de Celestia.

Crea un rollup soberano, que recoge transacciones en bloques y las publica en Celestia para obtener consenso y disponibilidad de datos.

El objetivo de Optimint es permitir a cualquiera diseñar y desplegar un rollup soberano en Celestia en minutos.

Además, mientras que Optimint te permite crear rollups soberanos en Celestia, actualmente no soporta pruebas de fraude aún y está por lo tanto funcionando en modo "pesimista", donde los nodos necesitarían volver a ejecutar las transacciones para comprobar la validez de la cadena (i.. un nodo completo). Además, Optimint actualmente soporta un solo secuenciador.

## Tutoriales

Los siguientes tutoriales te ayudarán a comenzar a construir aplicaciones de Cosmos-SDK que se conectan a la disponibilidad de datos de Celestia Layer a través de Optimint. Nosotros llamamos a esas cadenas Rollups Soberanas.

Puedes empezar con los siguientes tutoriales:

- [Wordle Game](./wordle.md)
- [Tutorial CosmWasm](./cosmwasm.md)
