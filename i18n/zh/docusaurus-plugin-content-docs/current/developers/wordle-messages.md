---
sidebar_label: 信息
---

# 信息

消息允许我们处理信息并将信息提交到特定模块。

从Cosmos SDK文档中，<a href=”https://docs.cosmos.network/master/building-modules/messages-and-queries.html#messages“>信息</a> 是：

> 在Cosmos SDK中，消息是包含的对象 在事务中触发状态转换。 每个cosmos SDK 模块定义了消息列表以及如何处理它们。

对于Wordle的消息，考虑到我们的初始设计，我们将用点火器发出2条信息。

* 第一个是：＜code＞SubmitWordle＜/code＞并且它只通过一天中的单词。
* 第二个是：＜code＞SubmitGuess＜/code＞并尝试猜测提交的 它还传递一个单词作为猜测。

有了这些初始设计，我们就可以开始创建消息了！

## 搭建信息

要创建＜code＞SubmitWordle</code＞消息，我们运行以下命令：

```sh
点燃脚手架消息并提交wordle word
```

这将创建＜code＞提交wordle</code＞消息，该消息将＜code＞word</code＞作为参数。

我们现在创建最后一条消息，`SubmitGuess`：

```sh
点燃脚手架消息并提交猜测词
```

这里我们使用＜code＞提交猜测＜/code＞传递一个单词作为猜测。
