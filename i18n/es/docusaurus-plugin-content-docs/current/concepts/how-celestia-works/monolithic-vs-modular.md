- - -
sidebar_label : Monolíticas vs. Bloques Modulares
- - -

# Monolíticas vs. Blockchains Modulares

Las Blockchains requieren [máquinas de estado replicadas](https://dl.acm.org/doi/abs/10.1145/98163.98167): los nodos en una red distribuida sin permiso aplican una secuencia ordenada de transacciones deterministas a un estado inicial resultando en un estado común final. Esto significa que las blockchains requieren las siguientes cuatro funciones:

- __Ejecución__ implica ejecutar transacciones que actualizan el estado correctamente. Por lo tanto, la ejecución debe asegurarse de que solo se ejecutan transacciones válidas, por ejemplo:   transacciones que resultan en transiciones automáticas de estado válidas.
- __El Acuerdo__ entraña un entorno para las capas de ejecución para verificar pruebas, resolver disputas de fraude y puente entre otras capas de ejecución.
- __El Consenso__ implica aceptar el orden de las transacciones.
- __La Disponibilidad de Datos__ (DA) implica que los datos de la transacción estén disponibles. Ten en cuenta que la ejecución, la solución y el consenso requieren DA.

Las blockchains tradicionales, es decir, _monolíticas blockchains_, implementan las cuatro funciones juntas en una sola capa de consenso base. El problema con las blockchains monolíticas es que la capa de consenso debe realizar muchas tareas diferentes y no pueden ser optimizadas en solo una de estas funciones. Como resultado, el paradigma monolítico limita el recorrido del sistema.

![Monolíticas VS Modulares](/img/concepts/monolithic-modular.png)

Como solución, las blockchains modulares desacoplan estas funciones entre capas especializadas como parte de una pila modular. Debido a la flexibilidad que proporciona la especialización, hay muchas posibilidades en las que se puede organizar esa pila. Por ejemplo, uno de estos arreglos es la separación de las cuatro funciones en tres capas especializadas.

La capa base consiste en DA y consenso y, por lo tanto, se refiere a como la capa de Consenso y DA (o por brevedad, la capa DA), mientras que tanto el establecimiento y la ejecución se mueven en la parte superior de sus propias capas. Como resultado, cada capa se puede especializar para realizar óptimamente sólo su función y, por lo tanto, aumentar el rendimiento del sistema. Además, este paradigm modular permite múltiples capas de ejecución, por ejemplo, [rollups](https://vitalik.ca/general/2021/01/05/rollup.html)para usar la misma capa de consenso y DA.
