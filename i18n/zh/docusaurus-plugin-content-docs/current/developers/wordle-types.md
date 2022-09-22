---
sidebar_label: 类型
---

# Wordle 类型

在接下来的步骤中，我们将通过我们创建的消息创建要使用的类型。

## 拼写 Wordle 类型

```sh
ignite scaffold map wordle word submitter --no-message
```

这种类型是一个名为 `Wordle` 的映射，具有两个值 `word` 和 `submitter`。 `submitter` 是提交者的地址提交 Wordle 的人。

第二种类型是 `Guess` 类型。 第二种类型是 ` Guess ` 类型。它允许我们保存对于提交解决方案的每个地址的最新猜测。

```sh
ignite scaffold map guess word submitter count --no-message
```

这里我们还存储 `count` 来计算这个地址submit了多少猜测
