---
sidebar_label: Consultar tu Rollup
---

# 💬 Di “¡hola mundo!”

Ahora, vamos a hacer que nuestra blockchain diga `¡Hola mundo!` y para ello necesitamos hacer los siguientes cambios:

- Modificar un archivo búfer de protocolo
- Crear una función de consulta keeper que devuelva datos
- Registrar una función de consulta

Los archivos de búfer de protocolo contienen llamadas proto RPC que definen consultas SDK de Cosmos y manejadores de mensajes, y mensajes proto que definen tipos de SDK de Cosmos. Las llamadas RPC también son responsables de exponer una API HTTP.

El Keeper es requerido para cada módulo Cosmos SDK y es una abstracción para modificar el estado de la blockchain. Las funciones de Keeper te permiten consultar o escribir el estado. Después de agregar una consulta a tu cadena, necesitas registrar la consulta. Sólo tendrás que registrar una consulta una vez.

El típico flujo de trabajo del desarrollador de blockchain de Cosmos es algo así:

- Comienza con los archivos prototipo para definir [mensajes](https://docs.cosmos.network/master/building-modules/msg-services.html) Cosmos SDK
- Definir y registrar [consultas](https://docs.cosmos.network/master/building-modules/query-services.html)
- Definir la lógica del manejador de mensajes
- Finalmente, implementar la lógica de estas consultas y manejadores de mensajes en las funciones del keeper

## ✋ Crea tu primera consulta

**Para esta parte del tutorial, abre una nueva ventana de terminal que no sea la misma en la que iniciaste la cadena.**

En tu nuevo terminal, `cd` en el directorio `gm` y ejecuta este comando para crear la consulta `gm`:

```bash
ignite scaffold query gm --response text
```

Respuesta:

```bash
modify proto/gm/query.proto
modify x/gm/client/cli/query.go
create x/gm/client/cli/query_gm.go
create x/gm/keeper/grpc_query_gm.go

🎉 Creada una consulta `gm`.
```

¿Qué ha pasado? `query` acepta el nombre de la consulta (`gm`), una lista opcional de parámetros de solicitud (vacía en este tutorial), y una lista opcional separada por comas de campos de respuesta con una bandera `--response` (`text` en este tutorial).

Ve al archivo `proto/gm/query.proto`, verás que `Gm` RPC ha sido añadido al servicio `Query`:

<!-- markdownlint-disable MD010 -->
<!-- markdownlint-disable MD013 -->
```protobuf
service Query {
  rpc Params(QueryParamsRequest) returns (QueryParamsResponse) {
    option (google.api.http).get = "/gm/gm/params";
  }
    rpc Gm(QueryGmRequest) returns (QueryGmResponse) {
        option (google.api.http).get = "/gm/gm/gm";
    }
}
```
<!-- markdownlint-enable MD013 -->
<!-- markdownlint-enable MD010 -->

El `Gm` RPC para el servicio `Query`:

- es responsable de devolver una `cadena de texto`
- Acepta los parámetros de la solicitud (`QueryGmRequest`)
- Devuelve la respuesta del tipo `QueryGmResponse`
- `option` define el endpoint que utiliza gRPC para generar una API HTTP

## 📨 Tipos de solicitud y respuesta

En el mismo archivo, encontraremos:

- `QueryGmRequest` está vacío porque no requiere parámetros
- `QueryGmResponse` contiene `texto` que se devuelve desde la cadena

```protobuf
message QueryGmRequest {
}

message QueryGmResponse {
  string text = 1;
}
```

## 👋 Función Gm keeper

El archivo `x/gm/keeper/grpc_query_gm.go` contiene la función keeper de `Gm` que gestiona la consulta y devuelve datos.

<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD010 -->
```go
func (k Keeper) Gm(goCtx context.Context, req *types.QueryGmRequest) (*types.QueryGmResponse, error) {
    if req == nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    ctx := sdk.UnwrapSDKContext(goCtx)
    _ = ctx
    return &types.QueryGmResponse{}, nil
}
```
<!-- markdownlint-enable MD010 -->
<!-- markdownlint-enable MD013 -->

La función `Gm` realiza las siguientes acciones:

- Hace una comprobación básica de la solicitud y lanza un error si es `nil`
- Almacena contexto en una variable `ctx` que contiene información sobre el entorno de la solicitud
- Devuelve la respuesta del tipo `QueryGmResponse`

Actualmente, la respuesta está vacía. Actualicemos la función de mantenimiento.

Nuestro archivo `query.proto` define que la respuesta acepta `texto`. Usa tu editor de texto para modificar la función keeper en `x/gm/keeper/grpc_query_gm.go`.

<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD010 -->
```go
func (k Keeper) Gm(goCtx context.Context, req *types.QueryGmRequest) (*types.QueryGmResponse, error) {
    if req == nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    ctx := sdk.UnwrapSDKContext(goCtx)
    _ = ctx
    return &types.QueryGmResponse{Text: "¡Hola mundo!"}, nil
}
```
<!-- markdownlint-enable MD010 -->
<!-- markdownlint-enable MD010 -->

## 🟢 Comienza tu Rollup Soberano

```bash
gmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"[http://localhost:26658](http://134.209.70.139:26658/)","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height 100783
```

El comando `query` tiene también la estructura `x/gm/client/cli/query_gm.go`que implementa una equivalencia CLI a la solicitud de gm y monta este comando en `x/gm/client/cli/query.go`. Ejecuta el siguiente comando y obtén la siguiente respuesta JSON:

```bash
gmd q gm gm
```

Respuesta:

```bash
texto: ¡Hola mundo!
```

![4.png](/img/gm/4.png)

¡Enhorabuena 🎉 has construido con éxito tu primer rollup y lo has consultado!
