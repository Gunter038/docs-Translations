---
sidebar_label: 运行 Wordle 链
---

# 运行 Wordle 链
<!-- markdownlint-disable MD013 -->

## 构建和运行 Wordle 链

在一个终端窗口中，运行以下命令：

```sh
ignite chain serve 
```

这将编译您刚刚编写的区块链代码，并创建一个创世文件和一些帐户供您使用。 一旦日志在输出中显示类似于以下日志的内容：

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5

🛠️  Building proto...
📦 Installing dependencies...
🛠️  Building the blockchain...
💿 Initializing the app...
🙂 Created account "alice" with address "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" with mnemonic: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Created account "bob" with address "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" with mnemonic: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

此命令创建了一个二进制二进制名为`worded` and `Alice` 和 `bob` 地址， 还有一个测试网水龙头 和 API接口。 你可以按CTRL-C确认退出程序 The reason for that is because we will run `wordled` binary separately with Rollmint flags added.

You can start the chain with rollmint configurations by running the following:

```sh
wordled start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```

请考虑以下方面：

> 请注意：在上述命令中，你需要将一个 Celestia 节点的 IP 地址传输给拥有Arabica开发网代币账户的 `base_url`。 按照此[教程](./node-tutorial.md)在 Celestia 节点部分设置 Celestia 轻节点并使用测试网水龙头资金创建钱包。

还需要注意：

> 重要提示：此外，在上述命令中，您需要在 Arabica开发网中指定最新的 区块高度为 `da_height` 您可以在[浏览器](https://explorer.celestia.observer/arabica) 中找到最新的区块编号 。 Also, for the flag `--rollmint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

在另一个窗口中，运行以下指令来submit a Wordle：

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async -y
```

> 注意：为了避免 任何超时错误，我们正在提交异步交易。 With Rollmint as a replacement to Tendermint, we need to wait for Celestia's Data-Availability network to ensure a block was included from Wordle, before proceeding to the next block. Currently, in Rollmint, the single aggregator is not moving forward with the next block production as long as it is trying to submit the current block to the DA network. 未来，在领先选择的情况下，区块产出和同步逻辑会显著提升 。

这将要求你确认交易，并发出以下信息。

```json
{
  "body":{
    "messages":[
       {
          "@type":"/YazzyYaz.wordle.wordle.MsgSubmitWordle",
          "creator":"cosmos17lk3fgutf00pd5s8zwz5fmefjsdv4wvzyg7d74",
          "word":"giant"
       }
    ],
    "memo":"",
    "timeout_height":"0",
    "extension_options":[
    ],
    "non_critical_extension_options":[
    ]
  },
  "auth_info":{
    "signer_infos":[
    ],
    "fee":{
       "amount":[
       ],
       "gas_limit":"200000",
       "payer":"",
       "granter":""
    }
  },
  "signatures":[
  ]
}
```

Cosmos-SDK将要求您在此确认交易：

```sh
confirm transaction before signing and broadcasting [y/N]:
```

输入 Y 进行确认。

然后你会得到一个带有交易哈希值的信息，如图所示：

```sh
code: 19
codespace: sdk
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128
```

注意，这并不意味着该交易包含在此区块中。 我们可以查询交易哈希值，以检查它是否已被包含在区块中或者是否有任何错误。

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

应该显示如下信息：

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

可以做些测试当做娱乐：

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

在确认交易后，采用上面同样的方法查询 `txhash`。 因为你提交的是整数，你会看到显示无效的错误信息。

现在试着输入以下：

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

在确认交易后，采用上面同样的方法查询 `txhash`。 After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted a word larger than 5 characters.

Now try to submit another wordle even though one was already submitted

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. You will get an error that a wordle has already been submitted for the day.

Now let’s try to guess a five letter word:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. Given you didn’t guess the correct word, it will increment the guess count for Bob’s account.

We can verify this by querying the list:

```sh
wordled q wordle list-guess --output json
```

This outputs all Guess objects submitted so far, with the index being today’s date and the address of the submitter.

With that, we implemented a basic example of Wordle using Cosmos-SDK and Ignite and Rollmint. Read on to how you can extend the code base.

## Extending in the Future

You can extend the codebase and improve this tutorial by checking out the repository [here](https://github.com/celestiaorg/wordle).

There are many ways this codebase can be extended:

1. You can improve messaging around when you guess the correct word.
2. You can hash the word prior to submitting it to the chain, ensuring the hashing is local so that it’s not revealed via front-running by others monitoring the plaintext string when it’s submitted on-chain.
3. You can improve the UI in terminal using a nice interface for Wordle. Some examples are [here](https://github.com/nimblebun/wordle-cli).
4. You can improve current date to stick to a specific timezone.
5. You can create a bot that submits a wordle every day at a specific time.
6. You can create a vue.js front-end with Ignite using example open-source repositories [here](https://github.com/yyx990803/vue-wordle) and [here](https://github.com/xudafeng/wordle).
