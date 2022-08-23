- - -
sidebar_label : 节点教程
- - -

# 使用Celestia节点获取和发送交易
<!-- markdownlint-disable MD013 -->

在本教程中，我们将介绍如何使用 Celestia 节点 API 通过命名空间 ID 从数据可用性层提交和检索消息。

This tutorial was assumes you are working in a Linux environment.

> 点击 [这里](./light-node-video.md)查看设置Celestia轻节点的视频教程。

## 硬件要求

The following minimum hardware requirements are recommended for running a light node:

- Memory: 2 GB RAM
- CPU: Single Core
- Disk: 5 GB SSD Storage
- Bandwidth: 56 Kbps for Download/56 Kbps for Upload

## 设置依赖项

First, make sure to update and upgrade the OS:

```sh
# If you are using the APT package manager
sudo apt update && sudo apt upgrade -y

# If you are using the YUM package manager
sudo yum update
```

These are essential packages that are necessary to execute many tasks like downloading files, compiling, and monitoring the node:

```sh
# If you are using the APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# If you are using the YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```

### Install Golang

Celestia-app and celestia-node are written in [Golang](https://go.dev/) so we must install Golang to build and run them.

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Now we need to add the `/usr/local/go/bin` directory to `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

To check if Go was installed correctly run:

```sh
go version
```

The output should be the version installed:

```sh
go version go1.18.2 linux/amd64
```

## Celestia 节点

### 安装Celestia节点

Install the celestia-node binary by running the following commands:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

### 实例化Celestia轻节点

现在，让我们实例化一个Celestia轻节点：

> 注意：所有Celestia节点类型都会暴露RPC端点，包括但不限于轻节点、桥接结点和全节点。

```sh
celestia light init
```

### 连接到公共核心端点

现在让我们运行 Celestia轻节点，并通过GRPC连接到示例公共核心端点。

> 注意：我们还鼓励您使用社区中提供的API终结点，比如Discord中有一些终结点。 这一个用于演示目的， 您可以在[这里](../nodes/mamaki-testnet#rpc-endpoints)找到 RPC 端点列表。

```sh
celestia light start --core.grpc http://<ip-address>:9090
```

For example, your command along with an RPC endpoint might look like this:

```sh
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090
```

### Keys and wallets

You can create your key for your node by running the following command:

```sh
make cel-key
```

Once you start the Light Node, a wallet key will be generated for you. Once you start the Light Node, a wallet key will be generated for you. You will need to fund that address with Mamaki Testnet tokens to pay for PayForData transactions.

You can find the address by running the following command in the `celestia-node` directory:

```sh
./cel-key list --node.type light --keyring-backend test
```

If you would like to fund your wallet with testnet tokens, head over to the Celestia Discord channel `#faucet`.

您可以在 Discord 中使用以下命令向您的钱包地址请求资金：

```console
$request <Wallet-Address>
```

当您创建钱包时，生成的`<Wallet-Address>`地址形如`celestia1******`。

您的钱包有了资金，您可以继续下一步。

## 节点 API 调用

打开另一个终端窗口以开始查询 API。 Open up another terminal window in order to begin querying the API. `celestia-node` exposes its RPC endpoint on port `26658` by default.

### 余额

现在，让我们查询我们的节点的默认账户余额 (这是我们先前生成的 `developer` 密钥关联的账户)：

```sh
curl -X GET http://127.0.0.1:26658/balance
```

它将输出以下内容：

```json
{
   "denom":"utia",
   "amount":"999995000000000"
}
```

这显示钱包中的余额。

### 获取块头

现在，让我们获取块头信息。

在这里，我们获取块1的块头：

```sh
curl -X GET http://127.0.0.1:26658/header/1
```

它将输出如下内容：

```json
{
   "header":{
      "version":{
         "block":11
      },
      "chain_id":"devnet-2",
      "height":1,
      "time":"2021-11-23T02:24:05.965728875Z",
      "last_block_id":{
         "hash":"",
         "parts":{
            "total":0,
            "hash":""
         }
      },
      "last_commit_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "data_hash":"7B578B351B1B0BBD70BB350019EBC964C44A140A37EF715B552A7F8F315ACD19",
      "validators_hash":"7F4EA93A134DEDBDA6A1FDD30D05760DD98A2B5FBA95DB3EFFFE7FCE4B361855",
      "next_validators_hash":"7F4EA93A134DEDBDA6A1FDD30D05760DD98A2B5FBA95DB3EFFFE7FCE4B361855",
      "consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
      "app_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "last_results_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "evidence_hash":"E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "proposer_address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E"
   },
   "commit":{
      "height":1,
      "round":1,
      "block_id":{
         "hash":"4632277C441CA6155C4374AC56048CF4CFE3CBB2476E07A548644435980D5E17",
         "parts":{
            "total":1,
            "hash":"BE1B789969938DB061B7723BE45C8A7B4B27192339A8E0A4E216775B2CB58D97"
         }
      },
      "signatures":[
         {
            "block_id_flag":2,
            "validator_address":"03F1044A6DF782189C7061FF89146B3D33608F17",
            "timestamp":"2021-11-23T11:53:56.123958759Z",
            "signature":"q/XF7Nc2ThcQgWfqi6LYOUEqLcU+sgVPY1nB2CLWdjRo80WwI6xy6QaYx1B0lmcKAkWR0YMxbc7vJzKF70qwBA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E",
            "timestamp":"2021-11-23T11:53:56.036027188Z",
            "signature":"0bbxeviCvgLlyIF47JoY1CMN2e/MhHtFzhdgV0zCM+P3qeO/rkh+0TSxoRVUB1MDvMCoA8hyffCw3amxympMAA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"31711F367349D1BD619BD0A39568A69614B8A048",
            "timestamp":"2021-11-23T11:53:56.135943844Z",
            "signature":"gT23rZ8+XcG5rQ9QS+uh+Wn5eAiJDVy8bh23Fb8hevnHsuO1er2892MXAohaLRF6TTWs/C6dItYph4B/k3b6DQ=="
         },
         {
            "block_id_flag":2,
            "validator_address":"5A253EC2A9AB20AFD48C7BED2AFCA53F5C80BCA6",
            "timestamp":"2021-11-23T11:53:56.081698742Z",
            "signature":"nMngIlJ7PPPtu0N20YwKhWt/H3/JrEKJC3rnS5KG/8J3IppTacuwjGDUqIuHpRlrD0MmWa1mlY+6+ulbytt2AA=="
         },
         {
            "block_id_flag":2,
            "validator_address":"79BEB39F4B396F9278DA03F1C97F9BE3B10B12D3",
            "timestamp":"2021-11-23T11:53:56.037896319Z",
            "signature":"wPAL/sK4YXSU7iRXl00GDLvi4IItVlkY2zRkxIUeV9VA3Tq8Tke6wGE0N6pXKmtMcKUMoyjm03RWHv4mrtj1BA=="
         },
         {
            "block_id_flag":1,
            "validator_address":"",
            "timestamp":"0001-01-01T00:00:00Z",
            "signature":null
         },
         {
            "block_id_flag":1,
            "validator_address":"",
            "timestamp":"0001-01-01T00:00:00Z",
            "signature":null
         },
         {
            "block_id_flag":2,
            "validator_address":"D345D62BBD18C301B843DF7C65F10E57AB17BD98",
            "timestamp":"2021-11-23T11:53:56.123499237Z",
            "signature":"puj5Epw3yPGjSsJk6aQI74S2prgL7+cuiEpCxXYzQxOi0kNIqh8UMZLYf+AVHDQNJXehSmrAK6+VsIt9NF0DDg=="
         },
         {
            "block_id_flag":2,
            "validator_address":"DEC2642E786A941511A401090D21621E7F08A36D",
            "timestamp":"2021-11-23T11:53:56.123136439Z",
            "signature":"J/lnFqARXj42Lfx5MGY0FO/wug+AyQRxTnQp1u1HyczvV+0hXXuk06Uosi61jQKgJG6JBJF2VidqA41/uKMEDw=="
         }
      ]
   },
   "validator_set":{
      "validators":[
         {
            "address":"03F1044A6DF782189C7061FF89146B3D33608F17",
            "pub_key":"sMcFgSIzlD77eZYgV7H4akyxoHCPc2oIQW05qWEB6b4=",
            "voting_power":5000,
            "proposer_priority":-40000
         },
         {
            "address":"04881EB0A0A4C1DB414C708249CEEC2FCF348F3E",
            "pub_key":"WdqZ8hoyc1HxZCJfQrAGKm2fFJZFg7PngPNGkA1RWXc=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"31711F367349D1BD619BD0A39568A69614B8A048",
            "pub_key":"pvwSRksq3ekXIiYK7IzjQJ870BxLqEma8zRr9n9VnXI=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"5A253EC2A9AB20AFD48C7BED2AFCA53F5C80BCA6",
            "pub_key":"RnmnTlKoKxNoh2TpohBDP3cKlx4ATiPOCvQFk/6xpUU=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"79BEB39F4B396F9278DA03F1C97F9BE3B10B12D3",
            "pub_key":"oh/N+GOIennBOAa/gPNCso1mDlqaHQNn7Op/X8opbeY=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"7F1105B7B219481810C49730AECB1A83036BCA3A",
            "pub_key":"Ow/AHP/Q3guPGymUKpvhnwae+QoCOpGztpVnP179IG8=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"87265CC17922E01497F40B701EC9F05373B83467",
            "pub_key":"MNi0Z+uNF5X1Bxj988IDXVl0BKUcLs7LItoMnX6dbg4=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"D345D62BBD18C301B843DF7C65F10E57AB17BD98",
            "pub_key":"4g3hhdyU4IIgWW/4sR0nax8bsC/M/fDbt1N8s/QanF8=",
            "voting_power":5000,
            "proposer_priority":5000
         },
         {
            "address":"DEC2642E786A941511A401090D21621E7F08A36D",
            "pub_key":"b+Vv6Lcp0bhIjOQncr+OYBHixCvU5+k34y4RqyvpluE=",
            "voting_power":5000,
            "proposer_priority":5000
         }
      ],
      "proposer":{
         "address":"03F1044A6DF782189C7061FF89146B3D33608F17",
         "pub_key":"sMcFgSIzlD77eZYgV7H4akyxoHCPc2oIQW05qWEB6b4=",
         "voting_power":5000,
         "proposer_priority":-40000
      }
   },
   "dah":{
      "row_roots":[
         "//////////7//////////uyLCVMJmAItYqbOqgHXm3OwHsq1xQiAX1kZV2Tgcobm",
         "/////////////////////ykyWNfDJZfigziZC5BN5L00KKuoyDPduwynDywauskL"
      ],
      "column_roots":[
         "//////////7//////////uyLCVMJmAItYqbOqgHXm3OwHsq1xQiAX1kZV2Tgcobm",
         "/////////////////////ykyWNfDJZfigziZC5BN5L00KKuoyDPduwynDywauskL"
      ]
   }
}
```

### 提交PFD交易

在此示例中，我们将向节点的`/submit_pfd` 端点提交 PayForData 交易。

注意事项：

- PFD指PayForData 消息
- 端点将`namespace_id`和`data`值作为输入。
- 命名空间 ID 应为8字节。
- 数据是原始消息的十六进制编码字节。
- `gas_limit`是用于交易的燃料限制

我们假定`namespace_id`为`0000010000000100`，以及`data`值为`f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5`。

您可以在[这里](https://go.dev/play/p/7ltvaj8lhRl)生成自己的`namespace_id`值和data值，我们创建了一个可用的Golang试验场。

运行以下命令：

```sh
curl -X POST -d '{"namespace_id": "0c204d39600fddd3",
  "data": "f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5",
  "gas_limit": 60000}' http://localhost:26658/submit_pfd
```

我们得到以下输出：

```json
{
   "height":2452,
   "txhash":"04A79AF9DA62FDB41ACD7D82EB0B9004AE4E4ED603B280A65816560B4F38A999",
   "data":"12200A1E2F7061796D656E742E4D7367506179466F7244617461526573706F6E7365",
   "raw_log":"[{\"msg_index\":0,\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"/payment.MsgPayForData\"}]},{\"type\":\"payfordata\",\"attributes\":[{\"key\":\"signer\",\"value\":\"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw\"},{\"key\":\"size\",\"value\":\"27\"}]}]}]",
   "logs":[
      {
         "msg_index":0,
         "events":[
            {
               "type":"message",
               "attributes":[
                  {
                     "key":"action",
                     "value":"/payment.MsgPayForData"
                  }
               ]
            },
            {
               "type":"payfordata",
               "attributes":[
                  {
                     "key":"signer",
                     "value":"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw"
                  },
                  {
                     "key":"size",
                     "value":"27"
                  }
               ]
            }
         ]
      }
   ],
   "events":[
      {
         "type":"coin_spent",
         "attributes":[
            {
               "key":"spender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"coin_received",
         "attributes":[
            {
               "key":"receiver",
               "value":"celestia17xpfvakm2amg962yls6f84z3kell8c5lpnjs3s",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"transfer",
         "attributes":[
            {
               "key":"recipient",
               "value":"celestia17xpfvakm2amg962yls6f84z3kell8c5lpnjs3s",
               "index":true
            },
            {
               "key":"sender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            },
            {
               "key":"amount",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"message",
         "attributes":[
            {
               "key":"sender",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"1utia",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia10jhckjxxymsufflglvypxscnmggetwh0gfasws/267",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"JMNihnKS/MtYJDprqEFGJuXh16tVADsDDxXaFFpvv2te57btl4LbiRzwRRiN2rvwkJ2zlAApu2ImT22MZBi5+A==",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia13zx48t96zauht0kpcn0kcfykc9wn8fehzcp9wq/1024",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"mIZIjbzN0/RQAlQN7TDWzqtey3vVBPe7IO3+IIDhJstIH8QU9vsHfl0Rql9qWMZQG4dM+77w9WmUcnCeS7edfw==",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"fee",
               "value":"",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"acc_seq",
               "value":"celestia1h36gnnwzneu0csqzn2waph5y983hf3dkaznlgz/0",
               "index":true
            }
         ]
      },
      {
         "type":"tx",
         "attributes":[
            {
               "key":"signature",
               "value":"sfy+XyP7iWU+V9q3zEIOWxbGihvhzUKRLNVeXP+a+5oRefIA/Pyqfm13A5NU9I27hhfvpqo9vhXW1waRgcI9OA==",
               "index":true
            }
         ]
      },
      {
         "type":"message",
         "attributes":[
            {
               "key":"action",
               "value":"/payment.MsgPayForData",
               "index":true
            }
         ]
      },
      {
         "type":"payfordata",
         "attributes":[
            {
               "key":"signer",
               "value":"celestia1vdjkcetnw35kzvtgxvmxwmnwwaaxuet4xp3hxut6dce8wctsdq6hjwfcxd5xvvmyddsh5mnvvaaq6776xw",
               "index":true
            },
            {
               "key":"size",
               "value":"27",
               "index":true
            }
         ]
      }
   ]
}
```

请您注意到上述输出， 它返回`高度`为`2452`，我们将它用于下一个命令。

#### Troubleshooting

If you encounter an error like:

```console
$ curl -X POST -d '{"namespace_id": "c14da9d459dc57f5", "data": "4f7a3f1aadd83255b8410fef4860c0cd2eba82e24a", "gas_limit": 60000}'  localhost:26658/submit_pfd
"rpc error: code = NotFound desc = account celestia1krkle0n547u0znz3unnln8paft2dq4z3rznv86 not found"
```

It is possible that the account you are trying to submit a PayForData from doesn't have testnet tokens yet. Ensure the testnet faucet has funded your account with tokens and then try again.

### 按区块高度获取命名空间相关的数据片段

在提交您的 PFD 交易后，该节点会返回包含PFD 交易的 块高度。 After submitting your PFD transaction, upon success, the node will return the block height for which the PFD transaction was included. You can then use that block height and the namespace ID with which you submitted your PFD transaction to get your message shares returned to you. In this example, the block height we got was 589 which we will use for the following command. 在这个例子中，我们得到的块高度是2452，我们将在下面的命令中使用它。

```sh
curl -X GET \
  http://localhost:26658/namespaced_shares/0c204d39600fddd3/height/2452
```

我们得到以下输出：

```json
{
   "shares":[
      "DCBNOWAP3dMb8fIMqAB+kQo7+LLmHaDya8oH73hxem6lQWX1AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=="
   ],
   "height":2452
}
```

这里的输出是base64编码数据。
