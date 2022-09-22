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

From the Cosmos-SDK docs, the [`bank`](https://docs.cosmos.network/master/modules/bank/) module is defined as the following:

> The bank module is responsible for handling multi-asset coin transfers between accounts and tracking special-case pseudo-transfers which must work differently with particular kinds of accounts (notably delegating/undelegating for vesting accounts). It exposes several interfaces with varying capabilities for secure interaction with other modules which must alter user balances. It exposes several interfaces with varying capabilities for secure interaction with other modules which must alter user balances.

We build the module with the `bank` dependency with the following command:

```sh
ignite scaffold module wordle --dep bank
```

This will scaffold the Wordle module to our Wordle Chain project.
