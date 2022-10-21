---
sidebar_label: Запросите свой накопительный пакет
---

# Скажи «доброе утро мир!»

Теперь мы собираемся заставить наш блокчейн говорить gm world, и для этого нам нужно внести следующие изменения:

- Изменить файл буфера протокола
- Создать функцию запроса хранителя, которая возвращает данные
- Зарегистрировать функцию запроса

Файлы буфера протокола содержат вызовы proto RPC, определяющие запросы Cosmos SDK и обработчики сообщений, а также proto сообщения, определяющие типы Cosmos SDK. Вызовы RPC также отвечают за раскрытие HTTP API.

Keeper требуется для каждого модуля Cosmos SDK и представляет собой абстракцию для изменения состояния блокчейна. Функции Keeper позволяют запрашивать или записывать состояние в систему. После добавления запроса в цепочку, вам необходимо зарегистрировать запрос. Вам нужно зарегистрировать запрос только один раз.

Типичный рабочий процесс разработчика блокчейна Cosmos выглядит так:

- Начинается с proto файлов для определения сообщений Cosmos SDK[](https://docs.cosmos.network/master/building-modules/msg-services.html)
- Определение и регистрация[ запросов](https://docs.cosmos.network/master/building-modules/query-services.html)
- Определение логики обработчика сообщений
- И наконец, внедрить логику этих запросов и обработчиков сообщений в функции keeper

## ✋ Создайте свой первый запрос

**Для этой части руководства откройте новое окно терминала, отличное от того, в котором вы начали цепочку.**

В новом терминале, перейдите в каталог `cd` и запустите команду `gm` для создания запроса `gm` :

```bash
ignite scaffold query gm -- текст ответа
```

Ответ:

```bash
modify proto/gm/query.proto
modify x/gm/client/cli/query.go
create x/gm/client/cli/query_gm.go
create x/gm/keeper/grpc_query_gm.go

🎉 Created a query `gm`.
```

What just happened? `query` accepts the name of the query (`gm`), an optional list of request parameters (empty in this tutorial), and an optional comma-separated list of response field with a `--response` flag (`text` in this tutorial).

Navigate to the `proto/gm/query.proto` file, you’ll see that `Gm` RPC has been added to the `Query` service:

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

The `Gm` RPC for the `Query` service:

- is responsible for returning a `text` string
- Accepts request parameters (`QueryGmRequest`)
- Returns response of type `QueryGmResponse`
- The `option` defines the endpoint that is used by gRPC to generate an HTTP API

## 📨 Query request and response types

In the same file, we will find:

- `QueryGmRequest` is empty because it does not require parameters
- `QueryGmResponse` contains `text` that is returned from the chain

```protobuf
message QueryGmRequest {
}

message QueryGmResponse {
  string text = 1;
}
```

## 👋 Gm keeper function

The `x/gm/keeper/grpc_query_gm.go` file contains the `Gm` keeper function that handles the query and returns data.

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

The `Gm` function performs the following actions:

- Makes a basic check on the request and throws an error if it’s `nil`
- Stores context in a `ctx` variable that contains information about the environment of the request
- Returns a response of type `QueryGmResponse`

Currently, the response is empty. Let’s update the keeper function.

Our `query.proto` file defines that the response accepts `text`. Use your text editor to modify the keeper function in `x/gm/keeper/grpc_query_gm.go` .

<!-- markdownlint-disable MD013 -->
<!-- markdownlint-disable MD010 -->
```go
func (k Keeper) Gm(goCtx context.Context, req *types.QueryGmRequest) (*types.QueryGmResponse, error) {
    if req == nil {
        return nil, status.Error(codes.InvalidArgument, "invalid request")
    }
    ctx := sdk.UnwrapSDKContext(goCtx)
    _ = ctx
    return &types.QueryGmResponse{Text: "gm world!"}, nil
}
```
<!-- markdownlint-enable MD010 -->
<!-- markdownlint-enable MD010 -->

## 🟢 Start your Sovereign Rollup

```bash
gmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"[http://localhost:26658](http://134.209.70.139:26658/)","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height 100783
```

The `query` command has also scaffolded `x/gm/client/cli/query_gm.go` that implements a CLI equivalent of the gm query and mounted this command in `x/gm/client/cli/query.go`. Run the following command and get the following JSON response:

```bash
gmd q gm gm
```

Response:

```bash
text: gm world!
```

![4.png](/img/gm/4.png)

Congratulations 🎉 you've successfully built your first rollup and queried it!
