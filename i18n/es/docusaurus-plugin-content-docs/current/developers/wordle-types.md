---
sidebar_label: Tipos
---

# Tipos de Wordle

Para los siguientes pasos, crearemos tipos para ser utilizados por los mensajes que creamos.

## Tipos de Scaffol Wordle

```sh
ignite scaffold map wordle word submitter --no-message
```

Este tipo es un mapa llamado `Wordle` con dos valores `word` y `submitter`. `submitter` es la dirección de la persona que envió la Wordle.

El segundo tipo es el tipo `Guess`. Nos permite almacenar la última conjetura para cada dirección que presentó una solución.

```sh
ignite scaffold map guess word submitter count --no-message
```

Aquí también estamos almacenando `count` para contar cuántos supuestos enviaron esta dirección.
