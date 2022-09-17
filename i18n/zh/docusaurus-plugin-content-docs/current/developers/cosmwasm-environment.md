---
sidebar_label: 设置网络环境
---

# 在 Celestia 上为 CosmWasm 设置环境
<!-- markdownlint-disable MD013 -->

现在 `wasmd` 二进制文件已经构建好了，我们需要设置一个本地网络，在 `wasmd` 和 Optimint 之间进行通信。

## 构建 Wasmd 网络

运行以下命令：

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

这使用 `wasmd` 二进制文件初始化了一个名为 `celeswasm` 的链。

运行以下命令帮助我们设置创世钱包：

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

如果需要，请确保存储生成的钱包输出以供以后参考。

现在，让我们添加一个创世钱包并使用它来更新我们的创世文件：

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

这样，我们就创建了一个本地网络创世文件。

我们可以设置一些更有用的命令：

```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```

## 启动 Wasmd 网络

我们可以运行以下命令来启动 `wasmd` 网络：

```sh
wasmd start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

请考虑以下方面：

> 请注意：在上述命令中，你需要将一个 Celestia 节点的 IP 地址传递给拥有 Mamaki 测试代币账户的 `base_url`。 按照此[教程](./node-tutorial.md)在 Celestia 节点部分设置 Celestia 轻节点并使用测试网水龙头资金创建钱包。

还请考虑：

> 重要提示：此外，在上述命令中，您需要指定最新的 `da_height` 在 Mamaki 测试网中的块高度。 您可以在[此处](https://testnet.mintscan.io/celestia-testnet)的资源管理器中找到最新的区块编号。 另外，对于标注-- `--optimint.namespace_id`，你可以用 [这里](https://go.dev/play/p/7ltvaj8lhRl) 的测试版生成一个随机的 Namespace ID 。

这样，我们已经启动了我们的 `wasmd` 网络！
