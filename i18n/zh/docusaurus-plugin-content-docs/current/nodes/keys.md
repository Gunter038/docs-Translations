- - -
sidebar_label : 密钥
- - -

# 使用 cel-key 实用程序

在 celestia-node 存储库中有一个名为的实用程序`cel-key`，它在后台使用 Cosmos-SDK 提供的密钥实用程序。 此实用程序可以用于`添加`、`删除`和管理任何DA节点的密钥 输入 `(bridge || full || light)`，或者只是一般的密钥键。

## 安装

您需要先下拉 `celestia-node` 仓库：

```sh
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

您可以使用以下任何一个命令来构建它：

```sh
# dumps binary in current working directory, accessible via `./cel-key`
make cel-key
```

或者

```sh
# installs binary in GOBIN path, accessible via `cel-key`
make install-key
```

出于本指南的目的，我们将使用该 `make cel-key` 命令。

## 生成 **桥接** 节点密钥的步骤

要为 Celestia 桥接节点生成密钥，请执行以下操作：

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

这将会加载密钥 <key_name> 到桥接节点的目录中。

## 生成 **全** 节点密钥的步骤

要为 Celestia 全节点生成密钥，请执行以下操作：

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

这将会加载密钥 <key_name> 到全节点的目录中。

## 生成 **轻** 节点密钥的步骤

要为 Celestia 轻节点生成密钥，请执行以下操作：

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

这将会加载密钥 <key_name> 到轻节点的目录中。

## 导出 **轻** 节点密钥的步骤

您可以从本地密钥环中以加密和 ASCII 保护格式导出私钥。

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

然后您可以使用 `celestia-appd` 导入您的密钥：

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
