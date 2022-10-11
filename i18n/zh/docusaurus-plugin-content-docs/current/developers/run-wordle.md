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

此命令创建了一个二进制二进制名为`worded` and `Alice` 和 `bob` 地址， 还有一个测试网水龙头 和 API接口。 你可以按CTRL-C确认退出程序 原因是我们将运行`wordled` 添加了Rollmint标志的二进制文件。

您可以通过以下方式启动rollmint配置的链条 运行以下命令：

```sh
单词开头--rollmint。聚合器true——rollmint。da_layer celestia--rollmint.da_config='{“base_url”：“http://XXX.XXX.XXX.XXX:26658“，”timeout“：6000000000，”gas_limit“：6000000}'-rollmint.namespace_id 000000000000FFFF-rollming.da_start_height XXXXX”
```

请考虑以下方面：

> 请注意：在上述命令中，你需要将一个 Celestia 节点的 IP 地址传输给拥有Arabica开发网代币账户的 `base_url`。 按照此[教程](./node-tutorial.md)在 Celestia 节点部分设置 Celestia 轻节点并使用测试网水龙头资金创建钱包。

还需要注意：

> 重要提示：此外，在上述命令中，您需要在 Arabica开发网中指定最新的 区块高度为 `da_height` 您可以在[浏览器](https://explorer.celestia.observer/arabica) 中找到最新的区块编号 。 此外，对于旗帜 `--薄荷糖。namespace_id`，可以使用 操场<a href=“https://go.dev/play/p/7ltvaj8lhRl“>这里</a>

在另一个窗口中，运行以下指令来submit a Wordle：

```sh
wordled tx wordle提交wordle巨人--来自alice--keyring后端测试--chain id wordle-b async-y
```

> 注意：为了避免 任何超时错误，我们正在提交异步交易。 随着Rollmint取代Tendermint，我们 需要等待Celestia的数据可用性网络来确保数据块 从Wordle中包含，然后继续下一个块。 目前， 在Rollmint中，单个聚合器不会继续执行下一个块 只要它试图将当前块提交给DA网络，就可以进行生产。 未来，在领先选择的情况下，区块产出和同步逻辑会显著提升 。

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
签署和广播前确认交易[y/N]：
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

现在尝试提交另一个wordle，即使其中一个已经提交

```sh
wordled tx wordle提交wordle meter--来自bob--keyring后端测试--chain id wordle-b async-y
```

提交交易并确认后，查询`txhash` 就跟你刚才做的一样。 你会得到一个错误，一个单词 已提交当天的。

现在让我们试着猜一个五个字母的单词：

```sh
wordled tx wordle提交猜测最少--来自bob--keyring后端测试--chain id wordle-b async-y
```

提交交易并确认后，查询`txhash` 就跟你刚才做的一样。 假设你没有猜对 也就是说，它将增加Bob帐户的猜测计数

我们可以通过查询列表来验证这一点：

```sh
wordled q wordle list guess--输出json
```

这将输出到目前为止提交的所有Guess对象，并带有索引 是今天的日期和提交者的地址。

通过这个，我们使用 Cosmos SDK、Ignite和Rollmint。 继续阅读如何扩展代码库。

## 在未来扩展

您可以通过检查来扩展代码库并改进本教程 存储库外<a href=“https://github.com/celestiaorg/wordle“>此处</a>。

可以通过多种方式扩展此代码库：

1. 当你猜到正确的单词时，你可以改进周围的信息传递。
2. 您可以在将单词提交到链之前对其进行散列， 确保哈希是本地的，这样就不会通过 在以下情况下由其他人监视纯文本字符串的前端运行 它是通过链接提交的。
3. 您可以使用一个漂亮的界面来改进终端中的UI 沃德尔。 一些例子[ here](https://github.com/nimblebun/wordle-cli).
4. 您可以改进当前日期以坚持特定时区。
5. 您可以创建一个每天在特定时间提交wordle的bot。
6. 你可以创建一个虚拟世界。使用示例开源的带有Ignite的js前端 存储库<a href=“https://github.com/yyx990803/vue-wordle“>这里</a>和<a href=”https://github.com/xudafeng/wordle“>此处</a>。
