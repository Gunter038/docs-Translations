---
sidebar_label: Arabica Devnet
---

# Arabica Devnet
<!-- markdownlint-disable MD013 -->

![arabica-devnet](/img/arabica-devnet.png)

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

Select the type of node you would like to run and follow the instructions on each respective page. Whenever you are asked to select the type of network you want to connect to in those guides, select `Arabica` in order to refer to the correct instructions on this page on how to connect to Arabica.

## RPC endpoints

There is a list of RPC endpoints you can use to connect to Arabica Devnet:

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Arabica Devnet faucet

> USING THIS FAUCET DOES NOT ENTITLE YOU TO ANY AIRDROP OR OTHER DISTRIBUTION OF MAINNET CELESTIA TOKENS. MAINNET CELESTIA TOKENS DO NOT CURRENTLY EXIST AND THERE ARE NO PUBLIC SALES OR OTHER PUBLIC DISTRIBUTIONS OF ANY MAINNET CELESTIA TOKENS.

You can request from Arabica Devnet Faucet on the #arabica-faucet channel on Celestia's Discord server with the following command:

```text
$request <CELESTIA-ADDRESS>
```

Where `<CELESTIA-ADDRESS>` is a `celestia1******` generated address.

> Note: Faucet has a limit of 10 tokens per week per address/Discord ID

## Explorers

There is an explorer you can use for Arabica:

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
