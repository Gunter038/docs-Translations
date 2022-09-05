- - -
sidebar_label: Tạo Celestia Testnet
- - -

# Hướng dẫn cài đặt mạng ứng dụng Celestia

Hướng dẫn này giúp khởi tạo một mạng thử nghiệm mới và thực hiện các bước chính xác như vậy với Celestia-App. Bạn chỉ nên làm theo hướng dẫn này, nếu bạn muốn thử nghiệm với Celestia Testnetwork riêng hoặc nếu bạn muốn thử nghiệm các tính năng mới nhằm xây dựng như là nhà phát triển cốt lõi.

## Các yêu cầu phần cứng

Bạn có thể làm theo các yêu cầu về phần cứng [ tại đây ](../nodes/validator-node.md#hardware-requirements).

## Thiết lập các phần phụ thuộc

Bạn có thể thiết lập các phần phụ thuộc bằng cách làm theo hướng dẫn [ tại đây ](./environment.md).

## Cài đặt ứng dụng Celestia

Bạn có thể cài đặt Ứng dụng Celestia bằng cách làm theo hướng dẫn [ tại đây ](./celestia-app.md).

## Spin Up A Celestia Testnet

Nếu bạn muốn tạo một testnet nhanh với bạn bè của mình, bạn có thể làm theo các bước sau. Trừ khi có ghi chú khác, những người muốn tham gia vào testnet này phải thực hiện đủ các bước.

### Tùy chọn: Đặt lại thư mục làm việc

Nếu trước đây bạn đã khởi tạo một thư mục làm việc cho ` celestia-appd `, bạn phải dọn sạch trước khi khởi tạo lại một thư mục mới. Bạn có thể làm vậy bằng cách chạy lệnh sau:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Khởi tạo một thư mục làm việc

Chạy lệnh sau:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* Giá trị chúng tôi sẽ sử dụng cho ` $ VALIDATOR_NAME ` là ` validator1 ` nhưng bạn nên chọn tên node của riêng mình.
* Giá trị chúng tôi sẽ sử dụng cho `$CHAIN_ID` is `testnet`. ` $ CHAIN_ID ` phải giữ nguyên cho tất cả mọi người tham gia vào mạng lưới này.

### Tạo ra một Key mới

Tiếp theo, chạy câu lệnh sau:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

Thao tác này sẽ tạo một key, mới với tên do bạn chọn. Lưu kết quả của lệnh này ở đâu đó; có thể sau này bạn sẽ cần địa chỉ được tạo ra từ đây. Ở đây, chúng tôi đặt giá trị của key ` $ KEY_NAME ` to ` validator ` để minh họa.

### Thêm KeyName cho tài khoản Genesis

Chạy lệnh sau:

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Here `$VALIDATOR_NAME` is the same key name as before; and `$AMOUNT` is something like `10000000000000000000000000utia`.

### Tùy chọn: Thêm các validators khác

Nếu những người tham gia khác trong testnet của bạn cũng muốn làm validators, lặp lại lệnh trên với số tiền cụ thể với các keys công khai của chúng.

Sau khi tất cả validators được thêm, tệp ` genesis.json ` sẽ được tạo. Bạn cần phải chia sẻ nó với tất cả các validators khác trong testnet để mọi người tiến hành bước sau.

Bạn có thể tìm `genesis.json` at `$HOME/.celestia-app/config/genesis.json`

### Tạo giao dịch Genesis cho chuỗi mới

Chạy lệnh sau:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

Điều này sẽ tạo giao dịch genesis cho chuỗi mới của bạn. Here `$STAKING_AMOUNT` should be at least `1000000000utia`. If you provide too much or too little, you will encounter an error when starting your node.

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
