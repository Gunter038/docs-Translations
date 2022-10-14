---
sidebar_label: Tipos
---

# Tipos de Wordle

Para los siguientes pasos, crearemos tipos para ser utilizados por los mensajes que creamos.

## Scaffolding Wordle Types

```sh
ignite scaffold map wordle word submitter --no-message
```

Este tipo es un mapa llamado `Wordle` con dos valores de `word` y `submitter`. `submitter` es la dirección de la persona que envió el Wordle.

El segundo tipo es el tipo `Guess`. Esto nos permite almacenar la última suposición para cada dirección que presentó una solución.

```sh
ignite scaffold map guess word submitter count --no-message
```

Here, we are also storing `count` to count how many guesses this address submitted.
