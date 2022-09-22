---
sidebar_label: Thiết lập Môi trường Mạng
---

# Thiết lập môi trường cho CosmWasm trên Celestia

Bây giờ binary ` wasmd` đã được tạo, chúng tôi cần thiết lập một mạng nội bộ giao tiếp giữa ` wasmd` và Optimint.

## Xây dựng mạng Wasmd

Chạy lệnh sau:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=celeswasm
wasmd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

Điều này khởi tạo một chuỗi được gọi là ` celeswasm ` with ` wasmd ` binary.

Lệnh sau giúp chúng tôi thiết lập tài khoản cho genesis:

```sh
KEY_NAME=celeswasm-key
wasmd keys add $KEY_NAME --keyring-backend test
```

Đảm bảo bạn lưu trữ đầu ra của ví được tạo để tham khảo sau này nếu cần.

Bây giờ, hãy thêm tài khoản genesis và sử dụng nó để cập nhật tệp genesis:

```sh
TOKEN_AMOUNT="10000000000000000000000000uwasm"
wasmd add-genesis-account $KEY_NAME $TOKEN_AMOUNT --keyring-backend test
STAKING_AMOUNT=1000000000uwasm
wasmd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID --keyring-backend test
```

Như vậy, chúng tôi đã tạo một tệp genesis mạng nội bộ.

Một số lệnh hữu ích hơn mà chúng ta có thể thiết lập:

<!-- markdownlint-disable MD013 -->
```sh
export NODE="--chain-id ${CHAIN_ID}"
export TXFLAG="--chain-id ${CHAIN_ID} --gas-prices 0uwasm --gas auto --gas-adjustment 1.3"
```
<!-- markdownlint-enable MD013 -->

## Khởi động mạng Wasmd

Chúng ta có thể chạy khởi động mạng `wasmd` như sau:

<!-- markdownlint-disable MD013 -->
```sh
wasmd start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Vui lòng xem xét:

> NOTE: In the above command, you need to pass a Celestia Node IP address to the `base_url` that has an account with Arabica Devnet tokens. Theo dõi hướng dẫn thiết lập Celestia Light Node và tạo ví với tiền từ vòi testnet [here ](./node-tutorial.md) trong phần Celestia Node.

Cũng vui lòng xem xét:

> IMPORTANT: Furthermore, in the above command, you need to specify the latest Block Height in Arabica Devnet for `da_height`. You can find the latest block number in the explorer [here](https://explorer.celestia.observer/arabica). Also, for the flag `--optimint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

Như vậy, chúng ta đã khởi động lại mạng `wasmd` của mình!
