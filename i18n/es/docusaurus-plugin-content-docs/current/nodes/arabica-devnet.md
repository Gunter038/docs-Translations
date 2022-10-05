---
sidebar_label: Arabica Devnet
---

# Arabica Devnet
<!-- markdownlint-disable MD013 -->

![arabica Devnet](/img/arabica-devnet.png)

Arabica Devnet es una nueva testnet de Celestia Labs que se centra exclusivamente en proporcionar a los desarrolladores un rendimiento mejorado y las últimas actualizaciones para probar sus aplicaciones y rollups.

Arabica no se centra en las pruebas de validador o nivel de consenso, más bien, para eso es para lo que se utiliza Mamaki Testnet. Si eres un validador, te recomendamos solo probar las operaciones de tu validador en Mamaki [aquí](./mamaki-testnet.md).

Con Arabica tienes implementadas las últimas actualizaciones de todos los productos de Celestia en ella, puede estar sujeta a muchos cambios. Por lo tanto, como una advertencia justa, Arabia puede romperse inesperadamente, pero dado que se actualizará continuamente, es una forma útil de seguir probando los últimos cambios en el software.

Los desarrolladores todavía pueden desplegar en Mamaki Testnet sus rollups soberanos si eligieron hacerlo, esto siempre se retrasará detrás de Arabica Devnet hasta que Mamaki sufra actualizaciones de Hardfork en coordinación con validadores.

## Integraciones

Esta guía contiene las secciones relevantes sobre cómo conectarse a Mamaki, dependiendo del tipo de nodo que esté ejecutando.

Tu mejor estrategia para participar es determinar primero qué nodo deseas ejecutar. Cada guía de nodo se enlazará a la red correspondiente para mostrarte cómo conectarse a ellos.

Tienes una lista de opciones sobre el tipo de nodos que puedes ejecutar para participar en Arábica:

Disponibilidad de Datos:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Selecciona el tipo de nodo que deseas ejecutar y sigue las instrucciones en cada página correspondiente. Cada vez que se le pida que seleccione el tipo de red a la que desea conectarse en esas guías, seleccione `Arabica` para referirse a las instrucciones correctas en esta página sobre cómo conectarse a Arabica.

## Endpoints RPC

Hay una lista de endpoints RPC que puedes utilizar para conectarte a Mamaki Testnet:

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Arabica Devnet faucet

> USAR ESTE FAUCER NO TE DA ACCESO A NINGÚN AIRDROP U OTRA DISTRIBUCIÓN DE TOKENS DE LA MAINNET DE CELESTIA. LOS TOKENS DE MAINNET DE CELESTIA ACTUALMENTE NO EXISTEN NI SE HACEN VENTAS PÚBLICAS U OTRAS DISTRIBUCIONES PÚBLICAS DE CUALQUIER TOKEN DE MAINNET DE CELESTIA.

Puedes solicitar desde Mamaki Testnet Faucet en el canal #faucet en el servidor de Discord de Celestia con el siguiente comando:

```text
$request <CELESTIA-ADDRESS>
```

Donde `<CELESTIA-ADDRESS>` es una dirección `celestia1******` generada.

> Nota: Faucet tiene un límite de 10 tokens por semana por dirección/Discord ID

## Exploradores

Hay un explorador que puedes usar para Arabica:

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
