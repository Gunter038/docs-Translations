---
sidebar_label: Mensajes
---

# Mensajes

Los mensajes nos permiten procesar y enviar información a nuestro módulo específico.

De los documentos Cosmos-SDK, [mensajes](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages) son:

> En el SDK de Cosmos, los mensajes son objetos que están contenidos en las transacciones para activar transiciones de estado. Cada módulo de Cosmos SDK define una lista de mensajes y cómo manejarlos.

Para los mensajes para Wordle, dado nuestro diseño inicial, haremos 2 mensajes con ignite.

* La primera es: `SubmitWordle` y solo pasa la Palabra del Día.
* El segundo es: `SubmitGuess` e intenta adivinar la palabra enviada. También pasa una palabra como adivinación.

Con estos diseños iniciales, ¡podemos empezar a crear nuestros mensajes!

## Andamiaje (Scaffolding) de un mensaje

Para crear el mensaje `SubmitWordle`, ejecutamos el siguiente comando:

```sh
ignite scaffold message submit-wordle word
```

Esto crea el mensaje `submit-wordle` que toma `word` como parámetro.

Ahora creamos el mensaje final, `SubmitAdguess`:

```sh
ignite scaffold message submit-guess word
```

Aquí estamos pasando una palabra como adivinación con `submit-guess`.
