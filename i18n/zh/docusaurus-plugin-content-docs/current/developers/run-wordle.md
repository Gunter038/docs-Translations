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

此命令创建了一个二进制二进制名为`worded` and `Alice` 和 `bob` 地址， 还有一个水龙头 和 API接口。 你可以按CTRL-C确定退出程序 原因是我们将单独运行 `worded` 双进制标记添加了 Optimint 标记。

您可以通过运行以下步骤优化配置启动区块链

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

请考虑以下方面：

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Arabica devnet tokens. 按照此[教程](./node-tutorial.md)在 Celestia 节点部分设置 Celestia 轻节点并使用测试网水龙头资金创建钱包。

还需要注意：

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Arabica Devnet for `da_height`. You can find the latest block number in the explorer [here](https://explorer.celestia.observer/arabica). 另外，对于标注-- `--optimint.namespace_id`，你可以用 [这里](https://go.dev/play/p/7ltvaj8lhRl) 的测试版生成一个随机的 Namespace ID 。

在另一个窗口中，运行以下指令来submit a Wordle：

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

> 注意：为了避免 任何超时错误，我们正在提交异步交易。 在进入下一个区块之前。使用 Optimint 替换 Tendermint ， 我们 需要等待Celestia的数据可用性网络来确保一个区块被包含在 Wordle 中 。 目前在Optimint, 只要单个聚合器试图向DA网络提交当前区块，它就不会生产下一个区块。 未来，在领先选择的情况下，区块产出和同步逻辑会显著提升 。

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

现在尝试提交另一个Wordle，即使已有一个Wordle已经提交

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

在提交交易并确认后，用此前相同的方式查询已提供的 `txhash` 。 您将会收到一个单词 已经提交白天的错误。

现在让我们来猜测一个五个字母的单词：

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

在提交交易并确认后，用此前相同的方式查询已提供的 `txhash` 。 After submitting the transactions and confirming, query the `txhash` given the same way you did above. Given you didn’t guess the correct word, it will increment the guess count for Bob’s account.

我们可以通过查询列表来验证：

```sh
wordled q wordle list-guess --output json
```

这将输出到目前为止提交的所有猜测对象，索引 为今天的日期和提交者的地址。

我们用 COSMOS-SDK、Ignite 和 Optimint 执行了一个基本的Wordle示例。 阅读如何扩展 代码基础。

## 未来的扩展

您可以通过检查 这里 <a> 的repository来扩展代码库并改进此教程。</p> 

<p spaces-before="0">
  这个代码库可以通过多种方式扩展：
</p>

<ol start="1">
  <li>
    你可以改进周围的消息，当你猜到正确的单词时。
  </li>
  
  <li>
    <p spaces-before="0">
      你可以将单词提交到链上之前对其进行哈希处理， 确保散列函数是本地的，当它是在链上提交的时候，就不会通过 其他人在监控明文字符串时抢先运行 。
    </p>
  </li>
  
  <li>
    <p spaces-before="0">
      您可以在终端中使用一个不错的 Wordle 界面来改进用户界面。 <a href="https://github.com/nimblebun/wordle-cli">这里</a>是一些示例。
    </p>
  </li>
  
  <li>
    <p spaces-before="0">
      您可以改进当前日期以保持特定时区。
    </p>
  </li>
  
  <li>
    您可以创建一个bot，每天在特定时间提交Wordle。
  </li>
  
  <li>
    你可以通过Ignite使用示例开源储存库 repositories <a href="https://github.com/yyx990803/vue-wordle">here</a> and <a href="https://github.com/xudafeng/wordle">here</a> 创建一个 vue.js 前端
  </li>
</ol>
