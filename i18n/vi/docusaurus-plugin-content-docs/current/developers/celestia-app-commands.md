- - -
Những câu lệnh CLI hữu ích
- - -

# Những câu lệnh CLI hữu ích

Xem tất cả sự lựa chọn:

```console
$ celestia-appd --help
Chạy celestia app

Ứng dụng:
  celestia-appd [lệnh]

Những câu lệnh có sẵn:
add-genesis-account Thêm tài khoản genesis vào genesis.json
  collect-gentxs      Thu thập txs genesis và xuất tệp genesis.json
  config              Tạo hoặc truy vấn tệp cấu hình CLI ứng dụng
  debug               Công cụ giúp gỡ lỗi ứng dụng của bạn
  export              Xuất trạng thái sang JSON
  gentx               Tạo một tx genesis mang tự ủy quyền
  help                Trợ giúp về bất kỳ lệnh khác
  init                Khởi tạo tệp cấu hình ứng dụng, p2p, genesis và trình xác thực riêng tư
  keys                Quản lý khóa ứng dụng của bạn
  migrate             Di chuyển genesis sang một phiên bản mục tiêu cụ thể
  query               Truy vấn lệnh con
  rollback            Trở lại trạng thái tendermint
 rollback            Trở lại trạng thái tendermint và cosmos-sdk 
  start               Chạy full node
  status              Truy vấn nút từ xa để biết trạng thái
  tendermint        Lệnh con Tendermint
  tx                         Lệnh con Transactions
 validate-genesis    Xác thực tệp genesis tại vị trí mặc định hoặc tại vị trí được truyền dưới dạng đối số  
 version             In thông tin phiên bản nhị phân của ứng dụng
```

## Tạo ví

```sh
celestia-appd config keyring-backend test
```

` keyring-backend ` định cấu hình backend của keyring, nơi lưu trữ các khóa.

Các tùy chọn là: ` os | file | kwallet | pass | test | memory `.

## Quản lý khóa

```sh
# liệt kê khóa
 celestia-appd keys list

# thêm khóa
celestia-appd keys add <KEY_NAME>

# xóa khóa
celestia-appd keys delete <KEY_NAME>

# đổi tên khóa
celestia-appd keys rename <CURRENT_KEY_NAME> <NEW_KEY_NAME>
```

### Nhập và xuất keys

Nhập khóa cá nhân được mã hóa và trang bị ASCII vào keybase nội bộ.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Lệnh ví dụ:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Bạn có thể xuất một private key từ keyring nội bộ ở dạng mã hóa và định dạng ASCII-armored:

```sh
celestia-appd keys export <KEY_NAME>

# sau đó bạn sẽ được nhắc đặt mật khẩu cho khóa cá nhân được mã hóa:
Nhập cụm mật khẩu để mã hóa khóa đã xuất:
```

Sau khi bạn đặt mật khẩu, khóa được mã hóa của bạn sẽ xuất hiện.

## Câu lệnh phụ cho mục đích truy vấn

Cách sử dụng:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

Để xem tất cả sự lựa chọn:

```sh
celestia-appd q --help
```

## Quản lý token

Xem số dư token:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Lệnh ví dụ:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Gửi các token từ một ví này đến một ví khác:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Lệnh ví dụ:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

Để xem tất cả sự lựa chọn:

```sh
celestia-appd tx bank send --help
```

## Quản trị

Bạn có thể bỏ phiếu cho một đề xuất quản trị với lệnh sau:

```sh
celestia-appd tx gov vote <proposal id> <yes or no> --from <wallet> --chain-id <chain-id>
```

## Nhận phần thưởng cho validator

Bạn có thể nhận phần thưởng validator với câu lệnh sau:

```sh
celestia-appd tx distribution withdraw-rewards <validator valoper>\
    --commission --from=<validator wallet> --chain-id <chain-id> --gas auto -y
```

## Ủy thác & rút tokens ủy thác

Bạn có thể `ủy thác` token của bạn cho một validator với câu lệnh sau:

```sh
celestia-appd tx staking delegate <validator valoper> <amount>\
    --from <wallet>  --chain-id <chain-id>
```

Bạn có thể rút token ủy thác cho một validator với câu lệnh `unbond`:

```sh
celestia-appd tx staking unbond <validator valoper> <amount>\
    --from <wallet>  --chain-id <chain-id>
```

## Unjail validator

Bạn có thể unjail validator với câu lệnh sau:

```sh
celestia-appd tx slashing unjail --from <validator wallet>\
    --chain-id<chain-id> --gas auto -y
```

## Cách xuất logs với SystemD

Bạn có thể xuất logs nếu bạn chạy service SystemD với những câu lệnh sau:

```sh
sudo journalctl -u <your systemd service> -S yesterday > node_logs.txt
sudo journalctl -u <your systemd service> -S today > node_logs.txt
# Kết quả câu lệnh này sẽ kéo dài 1 triệu dòng!
sudo journalctl -u <your systemd service> -n 1000000 > node_logs.txt
```
