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

Linux, particularmente Ubuntu 20.04 LTS, es el más probado. Potentially compatible with other OSs, but they are currently untested. Some of the cryptography libraries used for erasure data are not guaranteed to work on other platforms.

### Syncing

Since we utilize Tendermint and the Cosmos-SDK, syncing the chain can be performed by any method that is supported by those libraries. This includes fast-sync, state sync, and quick sync.

### Notable exceptions relative to other blockchains

Relative to other Tendermint based chains, Celestia will have significantly longer blocktimes of around 30* seconds. The reason behind this block time is to optimize the bandwidth used by light clients that are sampling the chain, and is not because we have modified Tendermint consensus in any meaningful way. Validators will likely download/upload relatively large blocks. It should be noted that while these blocks are large, very little typical blockchain state execution is actually occurring on Celestia. Meaning that the bandwidth requirements will likely be larger than that of a typical Cosmos-SDK based blockchain full node, the computing requirements should be similar in magnitude.

*Subject to Change
