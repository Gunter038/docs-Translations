---
sidebar_label: 信息
---

# 信息

消息允许我们处理信息并将信息提交到特定模块。

从 Cosmos-SDK 文档， [消息](https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages) 是：

> 在Cosmos SDK中，信息是在触发状态转换的交易中包含的对象。 每个Cosmos SDK模块定义一个信息列表以及如何处理信息。

对于Wordle的信息，考虑到我们的初步设计，我们将与Ignite制作2则信息。

* 第一个是： `SubmitWord` 并且它只通过当天的Wordle。
* 第二个是： `提交人` 并试图猜测提交的Wordle。 它还传递一个单词作为猜测。

有了这些初始设计，我们就可以开始创建信息了！

## 搭建信息

要创建信息`SubmitWordle` ，我们运行以下命令：

```sh
ignite scaffold message submit-wordle word
```

这将创建`submit wordle`消息，该消息将`word`>作为参数

我们现在创建最后一条消息，`SubmitGuess`：

```sh
点燃脚手架消息提交猜测词
```

这里，我们用` submit-guess `传递一个单词作为猜测。
