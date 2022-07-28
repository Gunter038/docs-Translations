- - -
sidebar_label : 创建 Celestia 测试网
- - -

# Celestia App 网络实例指南

本指南旨在帮助实例化一个新的测试网络，并遵循正确的步骤来使用 Celestia-App。 如果您想试验自己的 Celestia 测试网络，或者如果您想测试新功能以作为核心开发人员构建，您应该只遵循本指南。

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

### 初始工作目录

运行以下命令：

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* 我们将使用`$VALIDATOR_NAME`都值是`validator1`但您应该选择自己的节点名称。
* 我们将用于 `$CHAIN_ID` 的值是 `testnet`。 The `$CHAIN_ID` must remain the same for everyone participating in this network.

### Create A New Key

Next, run the following command:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

This will create a new key, with a name of your choosing. Save the output of this command somewhere; you'll need the address generated here later. Here, we set the value of our key `$KEY_NAME` to `validator` for demonstration.

### Add Genesis Account KeyName

Run the following command:

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Here `$VALIDATOR_NAME` is the same key name as before; and `$AMOUNT` is something like `10000000000000000000000000utia`.

### Optional: Adding Other Validators

If other participants in your testnet also want to be validators, repeat the command above with the specific amount for their public keys.

Once all the validators are added, the `genesis.json` file is created. You need to share it with all other validators in your testnet in order for everyone to proceed with the following step.

You can find the `genesis.json` at `$HOME/.celestia-appd/config/genesis.json`

### Create the Genesis Transaction For New Chain

Run the following command:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

This will create the genesis transaction for your new chain. Here `$STAKING_AMOUNT` should be at least `1000000000utia`. If you provide too much or too little, you will encounter an error when starting your node.

You will find the generated gentx JSON file inside `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Note: If you have other validators in your network, they need to also run the above command with the `genesis.json` file you shared with them in the previous step.

### Creating the Genesis JSON File

Once all participants have submitted their gentx JSON files to you, you will pull all those gentx files inside the following directory: `$HOME/.celestia-appd/config/gentx` and use them to create the final `genesis.json` file.

Once you added the gentx files of all the particpants, run the following command:

```sh
celestia-appd collect-gentxs 
```

This command will look for the gentx files in this repo which should be moved to the following directory `$HOME/.celestia-app/config/gentx`.

It will update the `genesis.json` file after in this location `$HOME/.celestia-app/config/genesis.json` which now includes the gentx of other participants.

You should then share this final `genesis.json` file with all the other particpants who must add it to their `$HOME/.celestia-app/config` directory.

Everyone must ensure that they replace their existing `genesis.json` file with this new one created.

### Modify Your Config File

Open the following file `$HOME/.celestia-app/config/config.toml` to modify it.

Inside the file, add the other participants by modifying the following line to include other participants as persistent peers:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

You can find `validator_address` by running the following command:

```sh
celestia-appd tendermint show-node-id
```

The output will be the hex-encoded `validator_address`. The default `port` is 26656.

### Instantiate the Network

You can start your node by running the following command:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
