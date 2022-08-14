- - -
sidebar_label : 创建 Celestia 测试网
- - -

# Celestia App 网络实例指南

本指南旨在帮助实例化一个新的测试网络，并遵循正确的步骤来使用 Celestia-App。 This guide is for helping instantiate a new testnetwork and following the correct steps to do so with Celestia-App. You should only follow this guide if you want to experiment with your own Celestia Testnetwork or if you want to test out new features to build as a core developer.

## 硬件要求

您可以在[这里](../nodes/validator-node.md#hardware-requirements)找到硬件要求。

## 设置依赖项

您可以按照[这里](./environment.md)的指南设置依赖项。

## Celestia App 安装

您可以按照[这里](./celestia-app.md)的指南安装 Celestia App。

## 启动 Celestia 测试网

如果您想与您的朋友建立一个快速测试网，您可以按照以下步骤进行操作。 除非另有说明，否则每一步都必须由想要参与此测试网的每个人完成。

### 可选：重置工作目录

如果您过去已经为 `celestia-appd` 初始化了一个工作目录， 您必须先清理才能重新初始化一个新目录。 您可以通过运行以下命令来执行此操作：

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### 初始化工作目录

运行以下命令：

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* 我们将使用的`$VALIDATOR_NAME`的值是`validator1`，但您应该选择自己的节点名称。
* 我们将用于 `$CHAIN_ID` 的值是 `testnet`。 The value we will use for `$CHAIN_ID` is `testnet`. The `$CHAIN_ID` must remain the same for everyone participating in this network.

### 创建一个新密钥

运行以下命令：

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

这将创建一个具有您选择的名称的新密钥。 将此命令的输出内容保存到某处；后续您将需要这里生成的地址。 This will create a new key, with a name of your choosing. Save the output of this command somewhere; you'll need the address generated here later. Here, we set the value of our key `$KEY_NAME` to `validator` for demonstration.

### 添加创世账户密钥名

运行以下命令：

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

这里`$VALIDATOR_NAME`是与之前相同的密钥名称; 而`$AMOUNT`形如`100000000000000000000000utia`。

### 可选：添加其他验证者

如果您的测试网中的其他参与者也想要成为验证者，重复上面的命令来获取他们具有一定余额的公钥。

添加了所有验证者后，`genesis.json`文件就被创建好了。 Once all the validators are added, the `genesis.json` file is created. You need to share it with all other validators in your testnet in order for everyone to proceed with the following step.

你可以在`$HOME/.celestia-appd/config/genesis.json`找到`genesis.json`

### 为新链创建创世交易

运行以下命令：

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

这将为您的新链创建创世交易。 这里 `$STAKING_AMOUNT` 至少应该是 `10000000000utia`。 如果您提供了太多或太少, 您在启动节点时会遇到错误。

您会在 `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json` 中找到生成的 gentx JSON 文件。

> 注意：如果在您的网络中有其他验证者，他们也需要基于您在上一步中分享的`genesis.json`文件，运行上述命令。

### 创建创世JSON文件

所有参与者都向您提交了他们的 gentx JSON 文件后，您需要把所有这些 gentx 文件放入以下目录中： `$HOME/.celestia-appd/config/gentx` 并使用它们来创建最终`genesis.json`文件。

添加所有参与者的 gentx 文件后，运行以下命令：

```sh
celestia-appd collect-gentxs 
```

此命令将查找此目录中的gentx文件，应该将此目录移动到以下位置`$HOME/.celestia-app/config/gentx`

它将在`genesis.json` 文件这个位置后更新 `$HOME/.elestia-app/config/genesis.json` 现在包括其他参与者的 gentx。 。

然后你应该分享这个最终的`genesis.json<code>。所有其他参与者都必须将其添加到他们的<0>$HOME/.celestia-app/config`目录下。

每个人都必须确保将他们现有的 `genesis.json` 文件替换为 这个新的文件。

### 修改您的配置文件

打开以下文件`$HOME/.celestia-app/config/config.toml`进行修改。

在文件中，通过修改以下行来添加其他参与者，以将其他参与者标记为持久对等点：

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

您可以通过运行以下命令找到 `validator_address`

```sh
celestia-appd tendermint show-node-id
```

输出将是十六进制编码的 `validator_address`。 默认的 `port` 是 26656。

### 启动网络

您可以通过运行以下命令来启动节点：

```sh
celestia-appd start
```

现在您可以使用新的 Celestia 测试网了！
