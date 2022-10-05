- - -
sidebar_label : Capa de disponibilidad de datos de Celestia
- - -

# Capa de disponibilidad de datos de Celestia

Celestia es una capa de disponibilidad de datos (DA) que proporciona una solución escalable al problema de disponibilidad de datos [](https://coinmarketcap.com/alexandria/article/what-is-data-availability). Debido a la naturaleza sin permiso de las redes blockchain, una capa DA debe proporcionar un mecanismo para la ejecución y liquidación de capas para comprobar de una manera minimizada si los datos de transacción están efectivamente disponibles.

Dos características clave de la capa DA de Celestia son [muestra de disponibilidad de datos](https://blog.celestia.org/celestia-mvp-release-data-availability-sampling-light-clients/) (DAS) y [los Nombres de Dominio de los árboles Merkle ](https://github.com/celestiaorg/nmt) (NMTs). Ambas características son nuevas soluciones de escalabilidad de la blockchain: DAS permite que los light nodes verifiquen la disponibilidad de datos sin necesidad de descargar un bloque completo; Los NMTs permiten la ejecución y liquidación de capas en Celestia para descargar transacciones que solo son relevantes para ellos.

## Muestreo de Disponibilidad de Datos (DAS)

En general, los light nodes solo descargan los encabezados que contienen compromisos (es decir, raíz de Merkle) de los datos del bloque (es decir, la lista de transacciones).

Para hacer posible la DAS Celestia utiliza un esquema de codificación Reed-Solomon de 2 dimensiones para codificar los datos del bloque: cada dato de bloque se divide en fragmentos k × k, ordenado en una matriz k × k, y extendido con datos de paridad en una matriz extendida 2k × 2k aplicando múltiples veces la codificación Reed-Salomon.

Entonces, se calculan las raíces de Merkle 4k separadas para las filas y columnas de la matriz extendida; la raíz Merkle de estas raíces de Merkle se utiliza como el compromiso de datos de bloque en la cabecera de bloques.

![Codificación Reed-Saloman (RS) 2D](/img/concepts/reed-solomon-encoding.png)

Para verificar que los datos están disponibles, los light nodes de Celestia están muestreando los fragmentos de datos 2k × 2k.

Cada light node elige aleatoriamente un conjunto de coordenadas únicas en la matriz extendida y consulta a los full nodes para los fragmentos de datos y pruebas de Merkle correspondientes en esas coordenadas. Si los light nodes reciben una respuesta válida para cada consulta de muestreo, entonces hay una [garantía de alta probabilidad](https://github.com/celestiaorg/celestia-node/issues/805#issuecomment-1150081075) de que todos los datos del bloque están disponibles.

Además, cada fragmento de datos recibido con una prueba de Merkle correcta es informado a la red. Como resultado, mientras los light nodes de Celestia muestreen suficientes fragmentos de datos (i.., al menos k × k fragmentos únicos), el bloque completo puede ser recuperado por full nodes honestos.

Para más detalles sobre DAS, echa un vistazo al [original paper](https://arxiv.org/abs/1809.09044).

### Escalabilidad

La DAS permite a Celestia escalar la capa DA. La DAS puede ser realizada por light nodes limitados a recursos, ya que cada light node solo muestra una pequeña porción de los datos del bloque. Cuantos más light nodes haya en la red, más datos podrán descargar y almacenar de forma conjunta.

Esto significa que aumentar el número de light nodes que realizan DAS permite bloques más grandes (i.., con más transacciones), sin dejar de mantener la función DAS para light nodes limitados a recursos. Sin embargo, para validar encabezados de bloques, los nodos de Celestia necesitan descargar las raíces intermedias de Merkle.

Para un tamaño de bloque de datos de n bytes, significa que cada light node debe descargar O(n) bytes. Por lo tanto, cualquier mejora en la capacidad de ancho de banda de los light nodes de Celestia tiene un efecto cuadrático en el recorrido de la capa DA de Celestia.

### Prueba de Fraude de Datos Extendidos Incorrectamente

El requisito para descargar las raíces intermedias de Merkle 4k es una consecuencia del uso de un esquema de codificación Reed-Salomon de 2 dimensiones. Alternativamente, el DAS podría ser diseñado con un estándar (i.e. de 1 dimensión) con codificación Reed-Salomon, donde los datos originales se dividen en fragmentos k y se extienden con k fragmentos adicionales de datos de paridad. Dado que el compromiso de datos de bloques es la raíz de Merkle de los fragmentos de datos resultantes de 2k, los light nodes ya no necesitan descargar O(n) bytes para validar las cabeceras de bloque.

La desventaja de la codificación estándar Reed-Salomon es tratar con los productores de bloques maliciosos que generan los datos extendidos incorrectamente.

Esto es posible ya que __Celestia no requiere que la mayoría del consenso (i.., los productores de bloques) sean honestos para garantizar la disponibilidad de datos.__ Por lo tanto, si los datos extendidos son inválidos, los datos originales podrían no ser recuperables, incluso si los light nodes están muestreando suficientes fragmentos únicos (i.., al menos k para una codificación estándar y k × k para una codificación de 2 dimensiones).

Como solución, la _Prueba de Fraude de Datos Extendidos Incorrectamente_ habilita a los light nodes para rechazar bloques con datos extendidos inválidos. Tales pruebas requieren reconstruir la codificación y verificar el desajuste de funciones. Con la codificación estándar Reed-Salomon, esto implica descargar los datos originales, por ejemplo, O(n) bytes. En resumen, con codificación Reed-Salomon de 2 dimensiones, solo se requieren O(n ) bytes ya que es suficiente para verificar solo una fila o una columna de la matriz extendida.

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

![Componentes principales de la App de Celestia](/img/concepts/celestia-app.png)

La App de Celestia se construye sobre [Celestia Core](https://github.com/celestiaorg/celestia-core), una versión modificada del [algoritmo de consenso de Tendermint](https://arxiv.org/abs/1807.04938). Entre los cambios más importantes en Tendermint, Celestia Core:

- Habilita la codificación de borrado de los datos de bloque (usando el esquema de codificación de 2 dimensiones Reed-Solomon).
- Reemplaza el árbol de Merkle utilizado por Tendermint para almacenar los datos de bloques con un [árbol de Merkle con nombre de dominio](https://github.com/celestiaorg/nmt) que habilita las capas anteriores (i.., ejecución y liquidación) para descargar sólo los datos necesarios (para más detalles, mira la sección de abajo que describe los casos de uso).

Para más detalles sobre los cambios en Tendermint, echa un vistazo a [ADRs](https://github.com/celestiaorg/celestia-core/tree/v0.34.x-celestia/docs/celestia-architecture). Ten en cuenta que los nodos de Celestia Core siguen utilizando la red p2p de Tendermint.

Del mismo modo, Celestia Core está conectado a Tendermint a la capa de aplicación (i.e. la máquina del estado) por [ABCI++](https://github.com/tendermint/tendermint/tree/master/spec/abci%2B%2B), una evolución importante de [ABCI](https://github.com/tendermint/tendermint/tree/master/spec/abci) (Aplicación de Interfaz de Blockchain).

La máquina de estado de la App de Celestia es necesaria para ejecutar la lógica de PoS y para habilitar la gobernanza de la capa DA.

Sin embargo, la App de Celestia es agnóstica -- la máquina de estado ni valida ni almacena los datos que pone a disposición la App de Celestia.
