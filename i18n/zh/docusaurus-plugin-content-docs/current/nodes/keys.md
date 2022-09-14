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

To generate a key for a Celestia bridge node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

This will load the key <key_name> into the directory of the bridge node.

## Steps for generating **full** node keys

To generate a key for a Celestia full node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

This will load the key <key_name> into the directory of the full node.

## Steps for generating **light** node keys

To generate a key for a Celestia light node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

This will load the key <key_name> into the directory of the light node.

## Steps for exporting **light** node keys

You can export a private key from the local keyring in encrypted and ASCII-armored format.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

You can then import your key with `celestia-appd`:

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
