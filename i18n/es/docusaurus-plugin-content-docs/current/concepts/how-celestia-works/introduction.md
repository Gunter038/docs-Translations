# Introducción

Celestia es una red blockchain modular cuyo objetivo es construir una [capa de disponibilidad de datos escalable](https://blog.celestia.org/celestia-a-scalable-general-purpose-data-availability-layer-for-decentralized-apps-and-trust-minimized-sidechains/), habilitando la próxima generación de arquitecturas escalables de blockchain - [blockchains modulares](https://celestia.org/learn/). Celestia es escalable por el [desacoplamiento de la ejecución del consenso](https://arxiv.org/abs/1905.09274) y la presentación de un nuevo primitivo, [muestreo de disponibilidad de datos](https://arxiv.org/abs/1809.09044).

El primero implica que Celestia solo es responsable de ordenar transacciones y garantizar su disponibilidad de datos; esto es similar a [reducir el consenso a la transmisión atómica](https://en.wikipedia.org/wiki/Atomic_broadcast#Equivalent_to_Consensus).

El último proporciona una solución eficiente para el [problema de disponibilidad de datos](https://coinmarketcap.com/alexandria/article/what-is-data-availability) solo requiriendo nodos ligeros limitados a recursos para mostrar un pequeño número de fragmentos aleatorios de cada bloque para verificar la disponibilidad de datos.

Curiosamente, más nodos ligeros que participan en el muestreo incrementan la cantidad de datos que la red puede manejar de forma segura permitiendo que el tamaño del bloque aumente sin aumentar igualmente el coste para verificar la cadena.
