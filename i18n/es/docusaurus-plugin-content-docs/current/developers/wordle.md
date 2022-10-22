---
sidebar_label: Resumen de Wordle
---

# Wordle App en Rollmint

![mamaki-testnet](/img/wordle.jpg)

Esta guía de tutoriales irá construyendo una app cosmos-sdk para Rollmint, la implementación de Sovereign-Rollup de Tendermint, para el popular juego [Wordle](https://www. nytimes. com/games/wordle/index. html).

Este tutorial explicará cómo configurar Rollmint en Ignite CLI y usarlo para construir el juego. El tutorial pasará por encima el diseño simple, y concluirá con futuras implementaciones e ideas para extender este código.

> NOTA: Este tutorial explorará el desarrollo con Rollmint, que todavía está en fase Alfa. Si te encuentras con errores, por favor escribe un ticket de Github Issue o háznoslo saber en nuestro Discord. Además, mientras que Optimint te permite crear rollups soberanos en Celestia, actualmente no soporta pruebas de fraude aún y está por lo tanto funcionando en modo "pesimista", donde los nodos necesitarían volver a ejecutar las transacciones para comprobar la validez de la cadena (p.e. un nodo completo). Además, Rollmint actualmente soporta un solo secuenciador.

## Pre-requisitos

Dado que este tutorial está dirigido a desarrolladores con experiencia en Cosmos-SDK, te recomendamos que revises los siguientes tutoriales en Ignite para entender todos los diferentes componentes de Cosmos-SDK antes de continuar con este tutorial.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Blog and Module Basics](https://docs.ignite.com/guide/blog)
* [Tutorial de Nameservice](https://docs.ignite.com/guide/nameservice)
* [Scavenger Hunt](https://docs.ignite.com/guide/scavenge)

No tienes que hacer esas guías para seguir este tutorial de Wordle, pero haciéndolo te ayuda a entender mejor la arquitectura de Cosmos-SDK.

## Implementación de diseño

Las reglas de Wordle son simples: tienes que adivinar la palabra del día.

Puntos clave a considerar:

* La palabra es de cinco letras.
* Tiene 6 suposiciones.
* Cada 24 horas hay una nueva palabra.

El GUI para Wordle te muestra algunos indicadores: un resaltado verde en una letra en una posición determinada significa que es la letra correcta para la Wordle en la posición correcta. Un resaltado amarillo significa que es una letra correcta para la palabra incluida en la posición incorrecta. Un resaltado gris significa que la letra no forma parte de la Palabra.

Para la simplicidad del diseño, evitaremos esas pistas, aunque hay maneras de extender este código base para implementar eso, que mostraremos al final.

En este diseño actual, implementamos las siguientes reglas:

* 1 Palabra puede ser enviada por día
* Cada dirección tendrá 6 intentos de adivinar la palabra
* Debe ser una palabra de cinco letras.
* Quien adivine la palabra correctamente antes de que se acaben sus 6 intentos obtenga un premio de 100 tokens WORDLE.

Analizaremos la arquitectura para lograr esto en la guía. Pero por ahora, empezaremos a configurar nuestro entorno de desarrollo.

## Tabla de contenidos para este Tutorial

El siguiente tutorial se divide en las siguientes secciones:

1. [Ignite y Chain Scaffolding](./scaffold-wordle.md)
2. [Instalando Rollmint](./install-rollmint.md)
3. [Módulos](./wordle-module.md)
4. [Mensajes](./wordle-messages.md)
5. [Tipos](./wordle-types.md)
6. [Keepers](./wordle-keeper.md)
7. [Ejecutando Wordle](./run-wordle.md)
