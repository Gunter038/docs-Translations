---
sidebar_label: 设置网络环境
---

# 在 Celestia 上为 CosmWasm 设置环境

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

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## 启动 Wasmd 网络

我们可以运行以下命令来启动 `wasmd` 网络：

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

请考虑以下方面：

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Arabica Devnet tokens. 按照此[教程](./node-tutorial.md)在 Celestia 节点部分设置 Celestia 轻节点并使用测试网水龙头资金创建钱包。

还请考虑：

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Arabica Devnet for `da_height`. You can find the latest block number in the explorer [here](https://explorer.celestia.observer/arabica). Also, for the flag `--optimint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

这样，我们已经启动了我们的 `wasmd` 网络！
