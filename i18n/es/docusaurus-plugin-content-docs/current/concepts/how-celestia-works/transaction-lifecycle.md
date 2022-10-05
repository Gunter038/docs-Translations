- - -
sidebar_label : Capa de disponibilidad de datos de Celestia
- - -

# El ciclo de vida de una transacción en la App de Celestia

Los usuarios solicitan a la aplicación de Celestia para que los datos estén disponibles enviando transacciones con `PayForData`. Cada una de esas transacciones consiste en la identidad del remitente, los datos que se pondrán a disposición también referido como el mensaje, el tamaño de los datos, el identificador del espacio de nombres y una firma. Cada productor de bloques transfiere múltiples transacciones de `PayForData` en un bloque.

Sin embargo, antes de proponer el bloque, el productor lo pasa a la máquina de estado vía ABCI++, donde cada transacción de `PayForData` está dividida en un mensaje con espacio de nombres (indicado por `Msg` en la figura de abajo), ej. los datos junto con el ID del espacio de nombres, y una transacción ejecutable (indicado por `e-Tx` en la figura de abajo) que no contiene los datos, pero sí un compromiso que puede ser utilizado más tarde para demostrar que los datos han estado efectivamente disponibles.

Así, los datos del bloque consisten en datos particionados en espacios de nombres y transacciones ejecutables. Ten en cuenta que solo estas transacciones son ejecutadas por la máquina del estado Celestia una vez que el bloque es confirmado.

![El ciclo de vida de una transacción en la App de Celestia](/img/concepts/tx-lifecycle.png)

A continuación, el productor de bloques añade al encabezado un compromiso de los datos del bloque. Como se describe [aquí](./data-availability-layer.md#fraud-proofs-of-incorrectly-extended-data), el compromiso es la raíz de Merkle de las raíces intermedias 4k (i.., una para cada fila y columna de la matriz extendida). Para calcular este compromiso, el productor de bloque realiza las siguientes operaciones:

- Divide las transacciones ejecutables y los datos del espacio de nombres en acciones. Cada parte consiste en algunos bytes con prefijo de un ID de espacio de nombres. Para este fin, las transacciones ejecutables están asociadas con un namespace reservado.
- Establece estas acciones en una matriz cuadrada (en fila). Ten en cuenta que las acciones se añaden a la próxima potencia de dos. El resultado cuadrado del tamaño k × k se conoce como los datos originales.
- Se extiende los datos originales a una matriz cuadrada 2k × 2k usando el esquema de codificación 2-dimensional Reed-Salomon descrito anteriormente. Las acciones extendidas (por ejemplo, que contienen datos de borrado) están asociadas con otro namespace reservado.
- Calcula un compromiso para cada fila y columna de la matriz extendida usando los NMTs descritos anteriormente.

Entonces el compromiso de los datos de bloque es la raíz de un árbol Merkle con las hojas las raíces de un bosque de subárboles Merkle con Namespace, una para cada fila y columna de la matriz extendida.

## Comprobando la Disponibilidad de Datos

![Red DA](/img/concepts/consensus-da.png)

Para mejorar la conectividad, el nodo Celestia aumenta la aplicación Celestia con una red libp2p separada.., la llamada _red DA_, que sirve solicitudes DAS.

Los light nodes conectados a un nodo Celestia en la red DA, escuchan encabezados de bloque extendidos (i.e. los encabezados del bloque junto con los metadatos DA relevantes, tales como la raíz intermedia de Merkle), y realizan DAS en los encabezados recibidos (i.., pedir trozos de datos aleatorios).

Tenga en cuenta que aunque es recomendable, El rendimiento de DAS es opcional -- los light nodes solo podían confiar en que los datos correspondientes a los compromisos en los encabezados de bloques fueron efectivamente puestos a disposición por la capa de Celestia DA. Además, los light nodes también pueden enviar transacciones a la aplicación Celestia, es decir, transacciones con `PayForData`.

Mientras realizas DAS para la cabecera de un bloque, cada de light node consulta a los Nodos de Celestia sobre el número de fragmentos de datos aleatorios de la matriz extendida y pruebas de Merkle correspondientes. Si todas las consultas tienen éxito, entonces el light node acepta la cabecera del bloque como válida (desde una perspectiva de DA).

Si al menos una de las consultas falla (i.e. o el fragmento de datos no es recibido o la prueba de Merkle no es válida), entonces el light node rechaza la cabecera del bloque y vuelve a intentarlo más tarde. El nuevo ensayo es necesario para tratar con falsos negativos, es decir, bloquear cabeceras rechazadas a pesar de que los datos del bloque están disponibles. Esto puede suceder debido a la congestión de la red, por ejemplo.

Alternativamente, los light nodes pueden aceptar un encabezado de bloque aunque los datos no están disponibles, es decir, un _falso positivo_. Esto es posible ya que la propiedad de solidez (i.e. si un light node honesto acepta un bloque como disponible, entonces al menos un full node honesto eventualmente tendrá los datos de todo el bloque) probablemente garantizado (para más detalles, eche un vistazo al [original paper](https://arxiv.org/abs/1809.09044)).

Afinando los parámetros de Celestia (p. ej. el número de fragmentos de datos muestreados por cada light node) la probabilidad de falsos positivos puede ser suficientemente reducida de tal manera que los productores de bloques no tengan ningún incentivo para retener los datos del bloque.
