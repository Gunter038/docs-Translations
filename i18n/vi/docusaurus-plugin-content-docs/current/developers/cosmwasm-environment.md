---
sidebar_label: Thiết lập Môi trường Mạng
---

# Thiết lập môi trường cho CosmWasm trên Celestia

Now the `wasmd` binary is built, we need to setup a local network that communicates between `wasmd` and Rollmint.

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
wasmd start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```
<!-- markdownlint-enable MD013 -->

Vui lòng xem xét:

> LƯU Ý: Trong lệnh trên, bạn cần chuyển một địa chỉ IP Celestia Node vào `base_url`. Địa chỉ phải có tài khoản với token testnet Mamaki. Theo dõi hướng dẫn thiết lập Celestia Light Node và tạo ví với token testnet [tại đây ](./node-tutorial.md) trong phần Celestia Node.

Cũng vui lòng xem xét:

> QUAN TRỌNG: Hơn nữa, trong lệnh trên, bạn cần chỉ định Block Height mới nhất trong Arabica Devnet cho `da_height`. Bạn có thể tìm thấy số khối mới nhất trong explorer [tại đây](https://explorer.celestia.observer/arabica). Also, for the flag `--rollmint.namespace_id`, you can generate a random Namespace ID using the playground [here](https://go.dev/play/p/7ltvaj8lhRl)

Như vậy, chúng ta đã khởi động lại mạng `wasmd` của mình!
