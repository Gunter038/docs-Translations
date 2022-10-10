---
sidebar_label: Chạy chuỗi Wordle
---

# Chạy chuỗi Wordle
<!-- markdownlint-disable MD013 -->

## Xây dựng và chạy chuỗi Wordle

Trong terminal, hãy chạy lệnh sau:

```sh
ignite chain serve 
```

Việc này sẽ biên dịch blockchain code bạn vừa viết và đồng thời tạo một file genesis và một vài tài khoản để bạn sử dụng. Một khi log cho thấy một số thứ trông như những log sau trong phần kết quả:

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

Đây là lệnh tạo một binary gọi là `wordled` và địa chỉ`alice` và `bob`, cùng với một vòi và API. Bạn có thể thoát khỏi chương trình với CTRL-C. The reason for that is because we will run `wordled` binary separately with Rollmint flags added.

You can start the chain with rollmint configurations by running the following:

```sh
wordled start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```

Hãy cân nhắc rằng:

> LƯU Ý: Trong lệnh trên, bạn cần chuyển một địa chỉ IP Celestia Node vào `base_url`. Địa chỉ phải có tài khoản với token testnet Arabica. Theo dõi hướng dẫn thiết lập Celestia Light Node và tạo ví với token testnet [tại đây](./node-tutorial.md) trong phần Celestia Node.

Đồng thời hãy cân nhắc rằng:

> QUAN TRỌNG: Hơn nữa, trong lệnh trên, bạn cần chỉ định Block Height mới nhất trong Arabica Devnet cho `da_height`. Bạn có thể tìm thấy số khối mới nhất trong explorer [tại đây](https://explorer.celestia.observer/arabica). Also, for the flag `--rollmint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

Trong một cửa số khác, chạy câu lệnh sau để gửi một Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async -y
```

> LƯU Ý: Chúng ta đang gửi một giao dịch không đồng bộ để tránh bất kỳ lỗi thời gian chờ nào. With Rollmint as a replacement to Tendermint, we need to wait for Celestia's Data-Availability network to ensure a block was included from Wordle, before proceeding to the next block. Currently, in Rollmint, the single aggregator is not moving forward with the next block production as long as it is trying to submit the current block to the DA network. Trong tương lai, với việc lựa chọn leader, quá trình sản xuất khối và logic đồng bộ được cải thiện đáng kể.

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

With that, we implemented a basic example of Wordle using Cosmos-SDK and Ignite and Rollmint. Đọc tiếp cách bạn có thể mở rộng code base.

## Mở rộng trong tương lai

Bạn có thể mở rộng codebase và cải thiện hướng dẫn này bằng cách kiểm tra ngoài kho lưu trữ [here](https://github.com/celestiaorg/wordle).

Có nhiều cách để mở rộng codebase:

1. Bạn có thể cải thiện khả năng nhắn tin khi đoán đúng từ.
2. Bạn có thể hash từ trước khi gửi nó vào chuỗi, đảm bảo hashing là nội bộ để không bị tiết lộ qua front-running bởi những người khác giám sát chuỗi văn bản khi chúng được gửi trực tuyến.
3. Bạn có thể cải thiện giao diện người dùng trong thiết bị đầu cuối bằng cách sử dụng một giao diện đẹp cho Wordle. Một số ví dụ tại [ here](https://github.com/nimblebun/wordle-cli).
4. Bạn có thể cải thiện ngày hiện tại để phù hợp với một múi giờ cụ thể.
5. Bạn có thể tạo một bot gửi từ ngữ mỗi ngày vào một thời điểm cụ thể.
6. Bạn có thể tạo giao diện người dùng vue.js với Ignite bằng kho lưu trữ mã nguồn mở mẫu [here](https://github.com/yyx990803/vue-wordle) và [here](https://github.com/xudafeng/wordle).
