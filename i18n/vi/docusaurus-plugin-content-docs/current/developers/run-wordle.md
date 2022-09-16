---
sidebar_label: Run The Wordle Chain
---

# Run the Wordle Chain
<!-- markdownlint-disable MD013 -->

## Building and Running Wordle Chain

In one terminal window, run the following command:

```sh
ignite chain serve 
```

This will compile the blockchain code you just wrote and also create a genesis file and some accounts for you to use. Once the log shows something like the following log in the output:

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

Here the command created a binary called `wordled` and the `alice` and `bob` addresses, along with a faucet and API. You are clear to exit the program with CTRL-C. The reason for that is because we will run `wordled` binary separately with Optimint flags added.

You can start the chain with optimint configurations by running the following:

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Please consider:

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Mamaki testnet tokens. Follow the tutorial for setting up a Celestia Light Node and creating a wallet with testnet faucet money [here](./node-tutorial.md) in the Celestia Node section.

Also please consider:

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Mamaki Testnet for `da_height`. You can find the latest block number in the explorer [here](https://testnet.mintscan.io/celestia-testnet). Also, for the flag `--optimint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

In another window, run the following to submit a Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

> LƯU Ý: Chúng ta đang gửi một giao dịch không đồng bộ để tránh bất kỳ lỗi thời gian chờ nào. Với Optimint thay thế cho Tendermint, chúng ta cần phải đợi mạng Dữ liệu-Khả dụng của Celestia để đảm bảo khối đã được đưa vào Wordle, trước khi chuyển sang khối tiếp theo. Hiện nay, trong Optimint, bộ tổng hợp duy nhất không tiếp tục với việc sản xuất khối tiếp theo miễn là nó đang cố gắng gửi khối hiện tại đến mạng DA. Trong tương lai, với việc lựa chọn leader, quá trình sản xuất khối và logic đồng bộ được cải thiện đáng kể.

Thao tác này sẽ yêu cầu bạn xác nhận giao dịch bằng thông báo sau:

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

Cosmos-SDK sẽ yêu cầu bạn xác nhận giao dịch tại đây:

```sh
xác nhận giao dịch trước khi ký và phát [y / N]:
```

Xác nhận với Y.

Sau đó, bạn sẽ nhận được phản hồi với một hàm hash giao dịch như được hiển thị ở đây:

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

Lưu ý, điều này không có nghĩa là giao dịch đã được đưa vào khối. Hãy truy vấn giao dịch hash để kiểm tra xem nó đã được đưa vào khối chưa hoặc nếu có bất kỳ lỗi nào không.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

Điều này sẽ hiển thị một đầu ra như sau:

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Kiểm tra một vài điều cho vui:

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

Sau khi xác nhận giao dịch, hãy truy vấn `txhash` đưa ra theo đúng cách bạn đã làm ở trên. Bạn sẽ thấy các phản hồi hiển thị lỗi không hợp lệ vì bạn đã gửi số nguyên.

Bây giờ cố gắng:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

Sau khi xác nhận giao dịch, hãy truy vấn `txhash` được cung cấp giống cách bạn đã làm ở trên. Bạn sẽ thấy các phản hồi hiển thị lỗi không hợp lệ vì bạn đã gửi một từ lớn hơn 5 ký tự.

Bây giờ hãy cố gắng gửi một từ khác mặc dù một từ đã được gửi

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

Sau khi gửi giao dịch và xác nhận, hãy truy vấn `txhash` đưa ra theo giống cách bạn đã làm ở trên. Bạn sẽ gặp một lỗi mà một từ ngữ đã được gửi trong ngày.

Bây giờ chúng ta hãy thử đoán một từ gồm năm chữ cái:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

Sau khi gửi giao dịch và xác nhận, hãy truy vấn `txhash ` đưa ra theo giống cách bạn đã làm ở trên. Vì bạn không đoán đúng từ đó, nó sẽ tăng số lượt đoán cho tài khoản của Bob.

Chúng tôi có thể xác minh điều này bằng cách truy vấn danh sách:

```sh
wordled q wordle list-guess --output json
```

Điều này xuất ra tất cả các Guess objects đã gửi cho đến nay, với mục lục là ngày hôm nay và địa chỉ của người gửi.

Như vậy, chúng tôi đã triển khai một ví dụ cơ bản về Wordle bằng cách sử dụng Cosmos-SDK và Ignite và Optimint. Đọc tiếp cách bạn có thể mở rộng code base.

## Mở rộng trong tương lai

Bạn có thể mở rộng codebase và cải thiện hướng dẫn này bằng cách kiểm tra ngoài kho lưu trữ [here](https://github.com/celestiaorg/wordle).

Có nhiều cách để mở rộng codebase:

1. Bạn có thể cải thiện khả năng nhắn tin khi đoán đúng từ.
2. Bạn có thể hash từ trước khi gửi nó vào chuỗi, đảm bảo hashing là nội bộ để không bị tiết lộ qua front-running bởi những người khác giám sát chuỗi văn bản khi chúng được gửi trực tuyến.
3. Bạn có thể cải thiện giao diện người dùng trong thiết bị đầu cuối bằng cách sử dụng một giao diện đẹp cho Wordle. Một số ví dụ tại [ here](https://github.com/nimblebun/wordle-cli).
4. Bạn có thể cải thiện ngày hiện tại để phù hợp với một múi giờ cụ thể.
5. Bạn có thể tạo một bot gửi từ ngữ mỗi ngày vào một thời điểm cụ thể.
6. Bạn có thể tạo giao diện người dùng vue.js với Ignite bằng kho lưu trữ mã nguồn mở mẫu [here](https://github.com/yyx990803/vue-wordle) và [here](https://github.com/xudafeng/wordle).
