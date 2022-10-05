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

Điều này sẽ tạo giao dịch genesis cho chuỗi mới của bạn. `$STAKING_AMOUNT` ít nhất nên là `1000000000utia`. Nếu bạn cung cấp quá nhiều hoặc quá ít, bạn sẽ gặp lỗi khi khởi chạy node.

Bạn sẽ tìm thấy gentx file JSON được tạo ở bên trong `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Lưu ý: Nếu bạn có các validators khác trong mạng của bạn, chúng cũng cần phải     chạy câu lệnh ở trên với file `genesis.json` mà bạn chia sẻ với chúng trong bước trước.

### Tạo File Genesis JSON

Sau khi tất cả những người tham gia đã gửi file gentx JSON của họ cho bạn, bạn sẽ kéo tất cả các file gentx đó vào bên trong thư mục sau:`$HOME/.celestia-appd/config/gentx` và sử dụng chúng để tạo file `genesis.json` chính thức cuối cùng.

Một khi bạn đã thêm file gentx của tất cả những người tham gia, chạy câu lệnh sau:

```sh
celestia-appd collect-gentxs
```

Lệnh này sẽ tìm kiếm các tệp gentx trong kho lưu trữ này,thứ sẽ được chuyển đến thư mục sau`$HOME/.celestia-app/config/gentx`.

Nó sẽ cập nhật file `genesis.json` sau đó trong `$HOME/.celestia-app/config/genesis.json`, thứ bây giờ sẽ bao gồm gentx của những người tham gia khác.

Bạn sau đó nên chia sẻ file `genesis.json` cuối cùng với tất cả những người tham gia còn lại, những người sẽ phải thêm nó vào thư mục `$HOME/.celestia-app/config`.

Mọi người nên bảo đảm rằng họ đã thay file `genesis.json` hiện tại của họ với file mới được tạo.

### Điều chỉnh File Config

Mở file sau `$HOME/.celestia-app/config/config.toml` để điều chỉnh.

Bên trong tệp, hãy thêm những người tham gia khác bằng cách sửa đổi dòng sau thành bao gồm những người tham gia khác như là các persistent peers:

```text
# Dấu phẩy sẽ tách rời danh sách các nodes để giữ kết nối ổn định đối với persistent_peer = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

Bạn có thể tìm `validator_address` bằng cách chạy lệnh sau:

```sh
celestia-appd tendermint show-node-id
```

Kết quả sẽ là `validator_address` mã hóa bằng hàm hex. `Cổng` mặc định sẽ là 26656.

### Khởi chạy mạng

Bạn có thể khởi chạy node bằng cách chạy lệnh sau:

```sh
celestia-appd start
```

Bây giờ, bạn đã có một Testnet Celestia mới để tương tác với!
