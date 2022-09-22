---
sidebar_label: 脚手架链
---

# 点燃和搭建单词链
<!-- markdownlint-disable MD013 -->

## Ignite

Ignite 是一个了不起的 CLI 工具，可以帮助我们开始为 cosmos-sdk 应用程序构建我们自己的区块链。 它提供了许多强大的工具和脚手架，用于添加消息、类型和模块，并提供了大量的 cosmos-sdk 库。

您可以在[此处](https://docs.ignite.com/)阅读有关 Ignite的更多信息。

要安装 Ignite，您可以在终端中运行此命令：

```sh
curl https://get.ignite.com/cli | bash
sudo mv ignite /usr/local/bin/
```

这会在您的本地机器上安装 Ignite CLI。 本教程使用的是 MacOS，但它应该适用于 Windows。 对于 Windows 用户，请查看有关 Windows 机器安装的 Ignite 文档。

现在，使用或打开一个新的终端会话刷新您的终端以 `source` 进行更改。

运行以下命令：

```sh
ignite --help
```

您应该会看到帮助命令的输出，这意味着 Ignite 已成功安装！

## 搭建 Wordle 链

现在，有趣的部分来了，创建一个新的区块链！ 使用 Ignite，该过程非常简单明了。

Ignite CLI 附带了几个脚手架命令，旨在通过创建构建区块链所需的一切来使开发更加简单。

首先，我们将使用 Ignite CLI 构建全新 Cosmos SDK 区块链的基础。 Ignite 最大限度地减少了您必须自己编写的区块链代码。 如果您来自 EVM 的工程师，请将 Ignite 视为 Foundry 或 Hardhat 的 Cosmos-SDK 版本，但专门用于构建区块链。

我们首先运行以下命令来为我们的新区块链 Wordle 设置我们的项目。

```sh
ignite scaffold chain github.com/YazzyYaz/wordle --no-module
```

此命令在您运行命令的本地目录中构建一个名为 `wordle` 的新链目录。 请注意，我们传递了` --no-module` 标志，这是因为我们将在之后创建模块。

## Wordle 目录

现在，是时候进入目录了：

```sh
cd wordle
```

在里面你会看到你的 cosmos-sdk 区块链的几个目录和架构。

| 文件/目录  | 用途                                                       |
| ------ | -------------------------------------------------------- |
| 应用程序/  | 将区块链连接在一起的文件。 最重要的文件是 `app.go`，其中包含区块链的类型定义以及创建和初始化它的函数。 |
| 命令/    | 负责编译二进制 CLI 的主包。                                         |
| 文档/    | 项目文档目录。 默认情况下，会生成 OpenAPI 规范。                            |
| 原型/    | 描述数据结构的协议缓冲文件。                                           |
| 测试工具/  | 用于测试的辅助函数。                                               |
| vue/   | 一个 Vue 3 网络应用程序模板。                                       |
| x/     | Cosmos SDK 模块和自定义模块。                                     |
| 配置.yml | 用于自定义开发链的配置文件。                                           |
| 自述文件   | 您的主权应用程序特定区块链项目的自述文件。                                    |

逐一复习不在本指南的范围内，但我们鼓励您在[此处](https://docs.ignite.com/kb)阅读相关内容。

大部分教程工作将在 `x` 目录中进行。
