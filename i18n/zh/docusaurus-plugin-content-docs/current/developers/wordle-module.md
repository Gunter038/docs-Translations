---
sidebar_label: 模块
---

# 创建 Wordle 模块

对于 Wordle 模块，我们可以添加 Cosmos-SDK 提供的依赖项。

在 Cosmos-SDK 文档中，[模块](https://docs.ignite.com/guide/nameservice#cosmos-sdk-modules)定义如下：

> 在 Cosmos SDK 区块链中，特定于应用程序的逻辑在单独的模块中实现。 模块使代码易于理解和重新使用。 每个模块都包含自己的消息和事务处理器，而 Cosmos SDK 负责将每条消息路由到各自的模块。

存在许多用于 slashing、validating、auth 的模块。

## 脚手架模块

我们将使用 `bank` 模块依赖项进行交易。

在 Cosmos-SDK 文档中，[`bank`](https://docs.cosmos.network/master/modules/bank/) 模块定义如下：

> Bank 模块负责处理账户之间的多资产代币转账，并跟踪特殊情况下的伪转账，这些伪转账必须与特定类型的账户不同（特别是对归属账户的委托/取消委托）。 它公开了几个具有不同功能的接口，用于与必须改变用户余额的其他模块进行安全交互。

我们使用以下命令构建具有 `bank` 依赖项的模块：

```sh
ignite scaffold module wordle --dep bank
```

这将为我们的 Wordle 链项目搭建 Wordle 模块。
