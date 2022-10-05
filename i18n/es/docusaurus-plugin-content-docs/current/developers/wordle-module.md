---
sidebar_label: Módulo
---

# Creando el módulo Wordle

Para el módulo Wordle, podemos añadir dependencias ofrecidas por Cosmos-SDK.

De los documentos Cosmos-SDK, un [módulo](https://docs.ignite.com/guide/nameservice#cosmos-sdk-modules) se define como lo siguiente:

> En una blockchain de Cosmos SDK, la lógica específica de la aplicación se implementa en módulos separados. Los módulos mantienen el código fácil de entender y reutilizar. Cada módulo contiene su propio mensaje y procesador de transacciones, mientras que el SDK de Cosmos es responsable de enrutar cada mensaje a su módulo correspondiente.

Existen muchos módulos para particionar, validar, autenticar.

## Andamiaje (Scaffolding) de un Módulo

Utilizaremos la dependencia del módulo `bank` para las transacciones.

De los documentos Cosmos-SDK, un módulo [`bank`](https://docs.cosmos.network/master/modules/bank/) se define como lo siguiente:

> El módulo bank es responsable de manejar transferencias de monedas de múltiples activos entre cuentas y el seguimiento de pseudo-transferencias especiales que deben funcionar de forma diferente con tipos particulares de cuentas (principalmente delegando/deseleccionando para cuentas de inversión). Expone varias interfaces con capacidades variables para una interacción segura con otros módulos que deben modificar el balance del usuario.

Construiremos el módulo con la dependencia del `bank` con el siguiente comando:

```sh
ignite scaffold module wordle --dep bank
```

Esto creará el andamiaje (scaffold) del módulo de Wordle a nuestro proyecto Wordle Chain.
