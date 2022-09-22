- - -
sidebar_label : Capa de disponibilidad de datos de Celestia
- - -

# Capa de disponibilidad de datos de Celestia

Celestia es una capa de disponibilidad de datos (DA) que proporciona una solución escalable al problema de disponibilidad de datos [](https://coinmarketcap.com/alexandria/article/what-is-data-availability). Debido a la naturaleza sin permiso de las redes blockchain, una capa DA debe proporcionar un mecanismo para la ejecución y liquidación de capas para comprobar de una manera minimizada si los datos de transacción están efectivamente disponibles.

Dos características clave de la capa DA de Celestia son [muestra de disponibilidad de datos](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) y [los Nombres de Dominio de los árboles Merkle ](https://github.com/celestiaorg/nmt) (NMTs). Ambas características son nuevas soluciones de escalabilidad de la blockchain: DAS permite que los nodos ligeros verifiquen la disponibilidad de datos sin necesidad de descargar un bloque completo; Los NMTs permiten la ejecución y liquidación de capas en Celestia para descargar transacciones que solo son relevantes para ellos.

## Muestreo de Disponibilidad de Datos (DAS)

En general, los nodos ligeros solo descargan los encabezados que contienen compromisos (es decir, raíz de Merkle) de los datos del bloque (es decir, la lista de transacciones).

Para hacer posible la DAS Celestia utiliza un esquema de codificación Reed-Solomon de 2 dimensiones para codificar los datos del bloque: cada dato de bloque se divide en fragmentos k × k, ordenado en una matriz k × k, y extendido con datos de paridad en una matriz extendida 2k × 2k aplicando múltiples veces la codificación Reed-Salomon.

Entonces, se calculan las raíces de Merkle 4k separadas para las filas y columnas de la matriz extendida; la raíz Merkle de estas raíces de Merkle se utiliza como el compromiso de datos de bloque en la cabecera de bloques.

![2D Reed-Soloman (RS) Encoding](/img/concepts/reed-solomon-encoding.png)

Para verificar que los datos están disponibles, los nodos ligeros de Celestia están muestreando los fragmentos de datos 2k × 2k.

Cada nodo ligero elige aleatoriamente un conjunto de coordenadas únicas en la matriz extendida y consulta a nodos completos para los fragmentos de datos y pruebas de Merkle correspondientes en esas coordenadas. Si los nodos ligeros reciben una respuesta válida para cada consulta de muestreo, entonces hay una [garantía de alta probabilidad](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) de que todos los datos del bloque están disponibles.

Además, cada fragmento de datos recibido con una prueba de Merkle correcta es informado a la red. Como resultado, mientras los nodos ligeros de Celestia muestreen suficientes fragmentos de datos (i.., al menos k × k fragmentos únicos), el bloque completo puede ser recuperado por nodos completos honestos.

Para más detalles sobre DAS, echa un vistazo al [original paper](https://arxiv.org/abs/1809.09044).

### Escalabilidad

La DAS permite a Celestia escalar la capa DA. La DAS puede ser realizada por nodos ligeros limitados a recursos, ya que cada nodo ligero solo muestra una pequeña porción de los datos del bloque. Cuantos más nodos ligeros haya en la red, más datos podrán descargar y almacenar de forma conjunta.

Esto significa que aumentar el número de nodos ligeros que realizan DAS permite bloques más grandes (i.., con más transacciones), sin dejar de mantener la función DAS para nodos ligeros limitados a recursos. Sin embargo, para validar encabezados de bloques, los nodos de Celestia necesitan descargar las raíces intermedias de Merkle.

Para un tamaño de bloque de datos de n bytes, significa que cada nodo ligero debe descargar O(n) bytes. Por lo tanto, cualquier mejora en la capacidad de ancho de banda de los nodos ligeros de Celestia tiene un efecto cuadrático en el recorrido de la capa DA de Celestia.

### Prueba de Fraude de Datos Extendidos Incorrectamente

El requisito para descargar las raíces intermedias de Merkle 4k es una consecuencia del uso de un esquema de codificación Reed-Salomon de 2 dimensiones. Alternativamente, el DAS podría ser diseñado con un estándar (i.e. de 1 dimensión) con codificación Reed-Salomon, donde los datos originales se dividen en fragmentos k y se extienden con k fragmentos adicionales de datos de paridad. Dado que el compromiso de datos de bloques es la raíz de Merkle de los fragmentos de datos resultantes de 2k, los nodos ligeros ya no necesitan descargar O(n) bytes para validar las cabeceras de bloque.

La desventaja de la codificación estándar Reed-Salomon es tratar con los productores de bloques maliciosos que generan los datos extendidos incorrectamente.

Esto es posible ya que __Celestia no requiere que la mayoría del consenso (i.., los productores de bloques) sean honestos para garantizar la disponibilidad de datos.__ Por lo tanto, si los datos extendidos son inválidos, los datos originales podrían no ser recuperables, incluso si los nodos ligeros están muestreando suficientes fragmentos únicos (i.., al menos k para una codificación estándar y k × k para una codificación de 2 dimensiones).

Como solución, la _Prueba de Fraude de Datos Extendidos Incorrectamente_ habilita a nodos ligeros para rechazar bloques con datos extendidos inválidos. Tales pruebas requieren reconstruir la codificación y verificar el desajuste de funciones. Con la codificación estándar Reed-Salomon, esto implica descargar los datos originales, por ejemplo, O(n) bytes. En resumen, con codificación Reed-Salomon de 2 dimensiones, solo se requieren O(n ) bytes ya que es suficiente para verificar solo una fila o una columna de la matriz extendida.

## Nombre de Dominio de Árboles de Merkle (NMTs)

Celestia fragmenta los datos del bloque en múltiples nombres de dominio, uno para cada aplicación (por ejemplo, rollup) usando la capa DA. Como resultado, cada aplicación necesita descargar solo sus propios datos y puede ignorar los datos de otras aplicaciones.

Para que esto funcione, la capa DA debe ser capaz de demostrar que los datos proporcionados están completos, i.., se retornan todos los datos de un espacio de nombres de dominio determinado. Con este fin, Celestia está utilizando los nombre de dominio de árboles Merkle (NMTs).

Un NMT es un árbol Merkle con las hojas ordenadas por los identificadores del nombre de dominio y la función hash modificada para que cada nodo en el árbol incluya el rango de espacios de nombres de dominio de todos sus descendientes. La siguiente figura muestra un ejemplo de un NMT con altura tres (es decir, ocho fragmentos de datos). Los datos se fragmentan en tres nombres de dominio.

![Nombre de Dominio de Árboles de Merkle (NMTs)](/img/concepts/nmt.png)

Cuando una aplicación solicita los datos para el nombre de dominio 2, la capa DA debe proporcionar los fragmentos de datos `D3`, `D4`, `D5`, y `D6` y los nodos `N2`, `N8` y `N7` como prueba (ten en cuenta que la aplicación ya tiene la raíz `N14` de el encabezado del bloque).

Como resultado, la aplicación es capaz de comprobar que los datos proporcionados son parte de los datos del bloque. Además, la aplicación puede verificar que se proporcionaron todos los datos para el nombre de dominio 2. Si la capa DA proporciona, por ejemplo, sólo los fragmentos de datos `D4` y `D5`, también debe proporcionar los nodos `N12` y `N11` como pruebas. Sin embargo, la aplicación puede identificar que los datos están incompletos comprobando el rango de nombre de dominio de los dos nodos, i.., tanto `N12` como `N11` tienen descendientes parte del nombre de dominio 2.

Para más detalles sobre DAS, echa un vistazo al [original paper](https://arxiv.org/abs/1905.09274).

## Construir una Blockchain PoS para DA

### Proporcionando Disponibilidad de Datos

La capa de Celestia DA consiste en una blockchain PoS. Celestia apunta a esta blockchain como la [App de Celestia](https://github.com/celestiaorg/celestia-app), una aplicación que proporciona transacciones para facilitar la capa DA y que se construye usando [Cosmos SDK](https://docs.cosmos.network/v0.44/). La siguiente figura muestra los componentes principales de la App de Celestia.

![Main components of Celestia App](/img/concepts/celestia-app.png)

La App de Celestia se construye sobre [Celestia Core](https://github.com/celestiaorg/celestia-core), una versión modificada del [algoritmo de consenso de Tendermint](https://arxiv.org/abs/1807.04938). Entre los cambios más importantes en Tendermint, Celestia Core:

- Habilita la codificación de borrado de los datos de bloque (usando el esquema de codificación de 2 dimensiones Reed-Solomon).
- Replaces the regular Merkle tree used by Tendermint to store block data with a [Namespaced Merkle tree](https://github.com/celestiaorg/nmt) that enables the above layers (i.e., execution and settlement) to only download the needed data (for more details, see the section below describing use cases).

For more details on the changes to Tendermint, take a look at the [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Notice that Celestia Core nodes are still using the Tendermint p2p network.

Similarly to Tendermint, Celestia Core is connected to the application layer (i.e., the state machine) by [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), a major evolution of [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Application Blockchain Interface).

The Celestia App state machine is necessary to execute the PoS logic and to enable the governance of the DA layer.

However, the Celestia App is data-agnostic -- the state machine neither validates nor stores the data that is made available by the Celestia App.
