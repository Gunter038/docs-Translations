- - -
sidebar_label : Hướng dẫn chạy Node
- - -

# Nhận và gửi các giao dịch với Celestia Node
<!-- markdownlint-enable MD013 -->

Trong hướng dẫn này, chúng ta sẽ cùng học cách sử dụng API Celestia Node để gửi và lấy tin nhắn từ lớp Data Availability bằng ID namespace của bạn.

Hướng dẫn này mặc định rằng bạn đang sử dụng môi trường Linux.

> Để xem video hướng dẫn cách thiết lập light node Celestia, bấm [vào đây](./light-node-video.md)

## Các yêu cầu phần cứng

Yêu cầu phần cứng tối thiểu bên dưới đây được khuyến nghị để chạy một light node:

- Memory: 2 GB RAM
- CPU: Single Core
- Disk: 5 GB SSD Storage
- Bandwidth: 56 Kbps đối với Download/56 Kbps đối với Upload

## Thiết lập các phần phụ thuộc

Đầu tiên, hãy bảo đảm đã cập nhật và nâng cấp OS:

```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt update && sudo apt upgrade -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum update
```

Chúng là những gói thiết yếu, cần thiết để thực thi nhiều tác vụ ví dụ như tải file, biên dịch, và quản lý node:

<!-- markdownlint-disable MD013 -->
```sh
# Nếu bạn đang sử dụng APT package manager
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Nếu bạn đang sử dụng YUM package manager
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

### Cài đặt Golang

Celestia-app và celestia-node được viết bằng ngôn ngữ lập trình [Golang](https://go.dev/) do vậy chúng ta phải cài đặt Golang để thiết lập và chạy nó.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Bây giờ chúng ta cần thêm thư mục `/usr/local/go/bin` vào `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >>$HOME/.bash_profile
source $HOME/.bash_profile
```

Để kiểm tra xem Go đã được cài đặt đúng chưa:

```sh
go version
```

Đầu ra phải là phiên bản được cài đặt:

```sh
go version go1.18.2 linux/amd64
```

## Celestia Node

### Cài đặt Node Celestia

Cài đặt celestia-node binary bằng cách chạy các lệnh sau:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

Xác minh rằng binary đang hoạt động và kiểm tra phiên bản bằng lệnh celestia:

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Khởi tạo Light Node Celestia

Bây giờ, hãy khởi tạo một node Celestia Light:

> Lưu ý: Điểm cuối RPC được hiển thị trong tất cả các loại Node Celestia chẳng hạn như Light, Bridge và Full Nodes.

```sh
celestia light init
```

### Kết nối với điểm cuối Public Core

Bây giờ chúng ta hãy chạy node Celestia Light với kết nối GRPC cho một ví dụ về Điểm cuối Public Core.

> Lưu ý: Bạn cũng được khuyến khích tìm một điểm cuối API do cộng đồng điều hành và lưu trữ nhiều trong Discord. Cái này được dùng cho mục đích minh họa. Bạn có thể tìm thấy danh sách các điểm cuối RPC [tại đây](/nodes/arabica-devnet.md#rpc-endpoints)

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

> Lưu ý: `Cổng RPC chính` mặc định sẽ là 9090, nên nếu bạn không chọn một port cụ thể      trong câu lệnh, mặc định sẽ là port 9090. You can use the flag to specify another port if you prefer.

Ví dụ: lệnh của bạn cùng với điểm cuối RPC có thể trông giống như sau:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Mã khóa và ví

Bạn có thể tạo mã khóa cho node của mình bằng cách chạy lệnh sau:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name> 
```
<!-- markdownlint-enable MD013 -->

Khi bạn khởi động Light Node, một mã khóa ví sẽ được tạo. Bạn sẽ cần nạp tiền cho địa chỉ ví đó bằng Arabica Devnet tokens nhằm thanh toán cho các giao dịch PayForData.

Bạn có thể tìm thấy địa chỉ bằng cách chạy lệnh sau trong thư mục `celestia-node`:

```sh
./cel-key list --node.type light --keyring-backend test
```

Nếu bạn muốn nạp vào ví của mình tokens testnet, hãy truy cập kênh Discord của Celestia `#arabica-faucet`.

Bạn có thể yêu cầu nạp tiền vào địa chỉ ví của mình bằng lệnh sau trong Discord:

```console
$request <Wallet-Address>
```

Where ` <Wallet-Address> ` is the ` celestia1 ****** ` được xuất ra khi bạn tạo ví.

Với ví được nạp tiền, bạn có thể chuyển sang bước tiếp theo.

## Node API Calls

Mở một cửa sổ đầu cuối khác để bắt đầu truy vấn API. `celestia-node` exposes its RPC endpoint on port `26658` by default.

### Số dư

Bây giờ, hãy truy vấn nodes của chúng ta để biết số dư trong tài khoản mặc định (là tài khoản được liên kết với khóa ` developer ` mà chúng ta đã tạo ra trước đó):

```sh
curl -X GET http://127.0.0.1:26658/balance
```

Nó sẽ xuất ra như sau:

```json
{
   "denom":"utia",
   "amount":"999995000000000"
}
```

Nó cho bạn thấy số dư trong ví đó.

### Nhận tiêu đề khối

Bây giờ, hãy lấy thông tin tiêu đề khối.

Ở đây chúng ta sẽ lấy tiêu đề từ Khối 1:

```sh
curl -X GET http://127.0.0.1:26658/header/1
```

Nó sẽ xuất ra như thế này:

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
 
Text
XPath: /pre[18]/code
File: node-tutorial.md
```

### Gửi giao dịch PFD

Trong ví dụ này, chúng tôi sẽ gửi một giao dịch PayForData tới điểm cuối ` / submit_pfd ` của node.

Một số điều cần xem xét:

- PFD là một thông điệp PayForData.
- Điểm cuối cũng nhận các giá trị ` namespace_id ` và ` data `.
- ID Namespace nên là là 8 byte.
- Data sẽ nằm trong bytes được mã hóa hex của raw message.
- `gas_limit` là giới hạn gas được sử dụng cho giao dịch

Chúng ta sẽ sử dụng `namespace_id` của `0000010000000100` và giá trị `data` của `f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5`.

Bạn có thể tạo `namespace_id` của riêng bạn, và giá trị data bằng cách sử dụng Playground Golang chúng ta đã tạo [tại đây](https://go.dev/play/p/7ltvaj8lhRl).

Chạy lệnh sau:

```sh
curl -X POST -d '{"namespace_id": "0c204d39600fddd3",
  "data": "f1f20ca8007e910a3bf8b2e61da0f26bca07ef78717a6ea54165f5",
  "gas_limit": 60000}' http://localhost:26658/submit_pfd
```

Chúng ta sẽ nhận được kết quả như sau:

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

Nếu bạn chú ý trong kết quả ở trên, nó trả về một `height` của `2452`, thứ chúng ta sẽ sử dụng trong câu lệnh sau.

#### Khắc phục sự cố

Nếu bạn gặp phải một lỗi như sau:

<!-- markdownlint-disable MD013 -->
```console
$ curl -X POST -d '{"namespace_id": "c14da9d459dc57f5", "data": "4f7a3f1aadd83255b8410fef4860c0cd2eba82e24a", "gas_limit": 60000}'  localhost:26658/submit_pfd
"rpc error: code = NotFound desc = account celestia1krkle0n547u0znz3unnln8paft2dq4z3rznv86 not found"
```
<!-- markdownlint-enable MD013 -->

Có thể tài khoản bạn đang cố gắng gửi giao dịch PayForData chưa có tokens testnet. Hãy bảo đảm rằng vòi testnet đã nạp vào tài khoản của bạn tokens và thử lại.

### Nhận các Namespaced Shares theo Block Height

Sau khi gửi giao dịch PFD của bạn, khi thành công, node sẽ gửi trở lại Block Height mà giao dịch PFD đã được thực hiện. Bạn có thể sử dụng Block Height đó và namespace ID mà bạn đã gửi cùng với giao dịch PFD để nhận lại message shares của bạn. Trong ví dụ sau, Block Height chúng ta sẽ nhận là 589 bằng cách sử dụng câu lệnh sau.

```sh
curl -X GET \
  http://localhost:26658/namespaced_shares/0c204d39600fddd3/height/2452
```

Chúng ta sẽ nhận được kết quả như sau:

```json
{
   "shares":[
      "DCBNOWAP3dMb8fIMqAB+kQo7+LLmHaDya8oH73hxem6lQWX1AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA=="
   ],
   "height":2452
}
```

Kết quả ở đây được mã hóa base64.
