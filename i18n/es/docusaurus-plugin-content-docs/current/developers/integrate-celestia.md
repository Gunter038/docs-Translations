---
sidebar_label: Integrar Celestia
---

# Integrar Celestia

> Este documento es para proveedores de servicios de terceros, como custodios y exploradores, integrando la red Celestia.

## Notas del proveedor de servicio de Celestia

Celestia es una cadena basada en Cosmos-SDK bastante estandar. Utilizamos la última versión de Tendermint y el Cosmos-SDK, con solo modificaciones menores de cada uno. Esto significa que estamos:

- Utilizando los módulos por defecto de Cosmos-SDK: auth, bank, distribution, staking, slashing, mint, crisis, ibchost, genutil, evidence, ibctransfer, params, gov (limitado en algunas capacidades de TBD), upgrade, vesting, feegrant, capability y payment.
- Usando los esquemas de claves digitales estándar proporcionados por el Cosmos-SDK y Tendermint, que son secp256k1 para las transacciones de usuario, y tm-ed25519 para firmar y verificar mensajes de consenso.

En cuanto a exactamente qué módulos usados están sujetos a cambios, Celestia pretende tener el mínimo posible.

### Custodia y Gestión de Claves

Celestia soporta muchos sistemas de gestión de claves ya existentes, ya que confiamos en las librerías Cosmos-SDK y Tendermint para firmar y verificar transacciones. [Documentación Cosmos-SDK](https://docs.cosmos.network/master/basics/accounts.html#keys-accounts-addresses-and-signatures)

### RPC y consulta

En celestia-app, solo los endpoints RPC estándar para Tendermint y el Cosmos-SDK están expuestos. Actualmente no sumamos o restamos ninguna funcionalidad principal, pero esto podría cambiar en el futuro. Lo mismo ocurre con la consulta de datos de la cadena.

En el nodo Celestia, el cliente del nodo de disponibilidad de datos, hay una API JSON-RPC que te permite interactuar directamente con la capa de disponibilidad de datos de Celestia. La guía se puede encontrar [aquí](https://docs.celestia.org/developers/node-tutorial).

### Compatibilidad

Linux, particularmente Ubuntu 20.04 LTS, es el más probado. Potencialmente compatible con otros sistemas operativos, pero actualmente no están probados. Algunas de las librerías de criptografía utilizadas para borrar datos no están garantizadas para funcionar en otras plataformas.

### Sincronizando

Dado que utilizamos Tendermint y el Cosmos-SDK, la sincronización de la cadena puede ser realizada por cualquier método soportado por esas librerías. Esto incluye fast-sync, state sync, y quick sync.

### Excepciones notables relativas a otras blockchains

En relación con otras cadenas basadas en Tendermint, Celestia tendrá significativamente tiempos de bloqueo más largos de unos 30* segundos. La razón detrás de este tiempo de bloqueo es para optimizar el ancho de banda utilizado por clientes ligeros que muestren la cadena, y no es porque hemos modificado el consenso de Tendermint de ninguna manera significativa. Es probable que los validadores descarguen/suban bloques relativamente grandes. Hay que tener en cuenta que aunque estos bloques son grandes, en Celestia actualmente se está produciendo muy poca ejecución de transacciones. Probablemente los requerimientos de ancho de banda sean mayores que los de un nodo de blockchain basado en Cosmos-SDK, los requisitos de cálculo deben ser similares en magnitud.

* Sujeto a cambios
