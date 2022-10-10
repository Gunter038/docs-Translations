---
sidebar_label: Resumen de Wordle
---

# Wordle App on Rollmint

![mamaki-testnet](/img/wordle.jpg)

This tutorial guide will go over building a cosmos-sdk app for Rollmint, the Sovereign-Rollup implementation of Tendermint, for the popular game [Wordle](https://www.nytimes.com/games/wordle/index.html).

This tutorial will go over how to setup Rollmint in the Ignite CLI and use it to build the game. El tutorial pasará por encima del diseño simple, y concluirá con futuras implementaciones e ideas para extender este código.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Si encuentras errores, por favor escribe un ticket de Github Issue o háznoslo saber en nuestro Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Pre-requisitos

Dado que este tutorial está dirigido a desarrolladores con experiencia en Cosmos-SDK, te recomendamos que revises los siguientes tutoriales en Ignite para entender todos los diferentes componentes de Cosmos-SDK antes de continuar con este tutorial.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Blog y módulos básicos](https://docs.ignite.com/guide/blog)
* [Tutorial de Nameservice](https://docs.ignite.com/guide/nameservice)
* [Búsqueda del Tesoro](https://docs.ignite.com/guide/scavenge)

No tienes que hacer esas guías para seguir este tutorial de Wordle, pero haciéndolo le ayuda a entender mejor la arquitectura de Cosmos-SDK.

## Implementación de diseño

Las reglas de Wordle son simples: tienes que adivinar la palabra del día.

Puntos clave a considerar:

* La palabra es de cinco letras.
* Tienes 6 supuestos.
* Cada 24 horas hay una nueva palabra.

El GUI para Wordle te muestra algunos indicadores: un resaltado verde en una letra en una posición determinada significa que es la letra correcta para la Wordle en la posición correcta. Un resaltado amarillo significa que es una letra correcta para la palabra incluida en la posición incorrecta. Un resaltado gris significa que la letra no forma parte de la Palabra.

Para la simplicidad del diseño, evitaremos esas pistas, aunque hay maneras de extender este código base para implementar eso, que mostraremos al final.

En este diseño actual, implementamos las siguientes reglas:

* Se puede enviar 1 Wordle por día
* Cada dirección tendrá 6 intentos de adivinar la palabra
* Debe ser una palabra de cinco letras.
* Quien adivine la palabra correctamente antes de que se acaben sus 6 intentos obtenga un premio de 100 tokens WORDLE.

Analizaremos la arquitectura para lograr esto en la guía. Pero por ahora, empezaremos a configurar nuestro entorno de desarrollo.

## Tabla de contenidos para este Tutorial

El siguiente tutorial se divide en las siguientes secciones:

1. [Ignite y Chain Scaffolding](./scaffold-wordle.md)
2. [Installing Rollmint](./install-rollmint.md)
3. [Módulos](./wordle-module.md)
4. [Mensajes](./wordle-messages.md)
5. [Tipos](./wordle-types.md)
6. [Keepers](./wordle-keeper.md)
7. [Ejecutando Wordle](./run-wordle.md)
