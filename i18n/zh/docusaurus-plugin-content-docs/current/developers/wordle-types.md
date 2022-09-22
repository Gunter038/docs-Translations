---
sidebar_label: Types
---

# Wordle 类型

在接下来的步骤中，我们将通过我们创建的消息创建要使用的类型。

## 拼写 Wordle 类型

```sh
ignite scaffold map wordle word submitter --no-message
```

This type is a map called `Wordle` with two values of `word` and `submitter`. `submitter` is the address of the person that submitted the Wordle. `submitter` is the address of the person that submitted the Wordle.

第二种类型是 `Guess` 类型。 第二种类型是 ` Guess ` 类型。它允许我们保存对于提交解决方案的每个地址的最新猜测。

```sh
ignite scaffold map guess word submitter count --no-message
```

这里我们还存储 `count` 来计算这个地址submit了多少猜测
