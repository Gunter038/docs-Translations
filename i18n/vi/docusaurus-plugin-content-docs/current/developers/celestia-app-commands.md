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

### Importing and exporting keys

Import an encrypted and ASCII-armored private key into the local keybase.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Example usage:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Export a private key from the local keyring in encrypted and ASCII-armored format:

```sh
celestia-appd keys export <KEY_NAME>

# you will then be prompted to set a password for the encrypted private key:
Enter passphrase to encrypt the exported key:
```

After you set a password, your encrypted key will be displayed.

## Querying subcommands

Usage:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

To see all options:

```sh
celestia-appd q --help
```

## Token management

Get token balances:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Example usage:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Transfer tokens from one wallet to another:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Example usage:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

To see options:

```sh
celestia-appd tx bank send --help
```

## Governance

You can vote on a governance proposal with the following command:

```sh
celestia-appd tx gov vote <proposal id> <yes or no> --from <wallet> --chain-id <chain-id>
```

## Claim validator rewards

You can claim your validator rewards with the following command:

```sh
celestia-appd tx distribution withdraw-rewards <validator valoper>\
    --commission --from=<validator wallet> --chain-id <chain-id> --gas auto -y
```

## Delegate & undelegate tokens

You can `delegate` your tokens to a validator with the following command:

```sh
celestia-appd tx staking delegate <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

You can undelegate tokens to a validator with the `unbond` command:

```sh
celestia-appd tx staking unbond <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

## Unjailing the validator

You can unjail your validator with the following command:

```sh
celestia-appd tx slashing unjail --from <validator wallet>\
    --chain-id <chain-id> --gas auto -y
```

## How to export logs with SystemD

You can export your logs if you are running a SystemD service with the following command:

```sh
sudo journalctl -u <your systemd service> -S yesterday > node_logs.txt
sudo journalctl -u <your systemd service> -S today > node_logs.txt
# This command outputs the last 1 million lines!
sudo journalctl -u <your systemd service> -n 1000000 > node_logs.txt
```
