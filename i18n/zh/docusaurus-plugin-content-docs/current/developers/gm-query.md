---
sidebar_label: 查询你的Rollup
---

# 💬 说“gm world!”

现在，我们要让我们的区块链说 `gm world `，为了做到这一点，我们需要做以下修改。

- 修改protocol buffer文件
- 创建一个返回数据的 Keeper 查询函数
- 注册一个查询函数

Protocol buffer文件包含定义了Cosmos SDK查询和消息处理程序的proto RPC调用，以及定义了Cosmos SDK类型的proto消息。 RPC调用也负责导出一个HTTP的API

每个Cosmos的SDK模块中都需要有Keeper，这个是一个用于修改区块链状态的抽象模块。 Keeper函数允许你查询和写入状态。 在你添加一个查询到你的链后，你需要注册这个查询 你只需要注册一次查询

一个典型的Cosmos区块链开发者的工作流应该像这样：

- 创建proto文件用于定义Cosmos SDK的 [消息内容](https://docs.cosmos.network/master/building-modules/msg-services.html)
- 定义并且注册 [查询](https://docs.cosmos.network/master/building-modules/query-services.html)
- 定义消息处理逻辑
- 最后，在keeper函数中实现这些查询和消息处理逻辑

## ✋ 创建你的第一个查询

**为了体验该教程，请在打开另外一个终端窗口，而不是你启动链的窗口。**

在你的新终端窗口, `cd` 进入 `gm` 目录 并且执行此命令 去创建 `gm` 查询:

```bash
ignite scaffold query gm --response text
```

返回结果：

```bash
modify proto/gm/query.proto
modify x/gm/client/cli/query.go
create x/gm/client/cli/query_gm.go
create x/gm/keeper/grpc_query_gm.go

🎉 Created a query `gm`.
```

刚刚发生了什么？ `query` 命令接受的参数有，查询名称 (`gm`), 一个可选的参数列表 (本教程中为空), 和一个可选的响应字段列表 `--response` (`text` 本教程中为)。

打开 `proto/gm/query.proto` 文件, 你可以看到 `Gm` RPC 被添加到 `Query` service中:

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

`Gm` RPC 在 `Query` service中的作用:

- 负责返回一个 `text` 字符串
- 接收请求参数 (`QueryGmRequest`)
- 返回类型为 `QueryGmResponse` 的响应
- `option` 定义了使用gRPC去生成一个HTTP API的端点

## 📨 查询请求和响应类型

在同一个文件中，我们可以看到

- `QueryGmRequest` 是空的因为它不需要参数
- `QueryGmResponse` 有一个 `text` 表示从链中返回的数据

```protobuf
message QueryGmRequest {
}

message QueryGmResponse {
  string text = 1;
}
```

## 👋 Gm keeper 函数

`x/gm/keeper/grpc_query_gm.go` 文件包含了 `Gm` keeper 函数用于处理请求和返回类型。

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

`Gm` 函数执行以下操作：

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
