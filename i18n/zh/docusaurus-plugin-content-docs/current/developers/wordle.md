---
sidebar_label: Wordle 概述
---

# Wordle App on Rollmint

![mamaki-测试网](/img/wordle.jpg)

This tutorial guide will go over building a cosmos-sdk app for Rollmint, the Sovereign-Rollup implementation of Tendermint, for the popular game [Wordle](https://www.nytimes.com/games/wordle/index.html).

This tutorial will go over how to setup Rollmint in the Ignite CLI and use it to build the game. 本教程将介绍简单的设计， 并以未来的实施和想法结束扩展这个代码库。

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. 如果您遇到错误，请写一个 Github 问题票或在我们的 Discord 中告诉我们。 Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## 前置条件

鉴于本教程面向有 Cosmos-SDK 经验的开发人员，我们建议您在继续本教程之前阅读 Ignite 中的以下教程以了解 Cosmos-SDK 中的所有不同组件。

* [你好世界](https://docs.ignite.com/guide/hello)
* [博客和模块基础](https://docs.ignite.com/guide/blog)
* [名称服务教程](https://docs.ignite.com/guide/nameservice)
* [寻宝游戏](https://docs.ignite.com/guide/scavenge)

您不必按照这些指南来学习此 Wordle 教程，但这样做可以帮助您更好地理解 Cosmos-SDK 的架构。

## 设计与实现

Wordle 的规则很简单：你必须猜当天的单词。

需要考虑的要点：

* 这个词是一个五个字母的词
* 你有 6 个猜测
* 每24小时就会出现一个新的单词

Wordle 的 GUI 向您显示了一些指标：某个位置的字母上的绿色突出显示表示该字母在正确位置是 Wordle 的正确字母。 黄色突出显示表示它是包含在错误位置的 Wordle 的正确字母。 灰色高亮表示该字母不是 Wordle 的一部分。

为了设计的简单性，我们将避免这些提示，尽管有一些方法可以扩展这个代码库来实现它，我们将在最后展示。

在当前的设计中，我们实现了以下规则：

* 每天可以提交 1 个单词
* 每个地址将有 6 次尝试猜测单词
* 它必须是一个五个字母的单词
* 在 6 次尝试结束之前正确猜出单词的人将获得 100 个 WORDLE 代币的奖励

我们将在指南中详细介绍架构以进一步实现这一点。 但是现在，我们将开始设置我们的开发环境。

## 本教程的目录

以下教程分为以下几个部分：

1. [点燃和链式脚手架](./scaffold-wordle.md)
2. [Installing Rollmint](./install-rollmint.md)
3. [模块](./wordle-module.md)
4. [留言](./wordle-messages.md)
5. [类型](./wordle-types.md)
6. [守护者](./wordle-keeper.md)
7. [运行单词](./run-wordle.md)
