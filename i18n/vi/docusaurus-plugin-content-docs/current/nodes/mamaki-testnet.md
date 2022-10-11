- - -
sidebar_label : Mamaki Testnet
- - -

# Mamaki Testnet

![mamaki-testnet](/img/mamaki.png)

Hướng dẫn này gồm các phần liên quan về cách kết nối với Mamaki, tùy thuộc vào loại node bạn đang chạy. Mamaki testnet được thiết kế để giúp các validators thử nghiệm cơ sở hạ tầng và phần mềm node đối với mạng test. Nhà phát triển được khuyến khích để triển khai rollup chuyên dụng của họ trên Mamaki, tuy nhiên, chúng tôi cũng khuyến khích sử dụng [Arabica Devnet](./arabica-devnet.md) để làm việc đó bởi vì nó được thiết kế cho mục đích phát triển.

Mamaki là một cột mốc của Celestia, cho phép mọi người có thể thử nghiệm cách tính năng cốt lõi trên mạng lưới. Đọc thông báo [here](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/).

Cách tốt nhất để tham gia là trước tiên xác định bạn muốn chạy node nào. Mỗi hướng dẫn node sẽ liên kết đến mạng liên quan để hướng dẫn bạn cách kết nối với chúng.

Bạn có một danh sách các tùy chọn về loại node bạn có thể chạy để tham gia Mamaki:

Đồng thuận:

* [Validator Node](./validator-node.md)
* [Consensus Full Node](./consensus-full-node.md)

Data Availability:

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Chọn loại node bạn muốn chạy và làm theo hướng dẫn trên mỗi trang tương ứng. Bất cứ khi nào bạn được yêu cầu chọn loại mạng bạn muốn kết nối trong các hướng dẫn, hãy chọn `Mamaki` để có thể tham khảo hướng dẫn chính xác trên trang này về cách kết nối với Mamaki.

## RPC Endpoints

Đây là danh sách các RPC endpoints bạn có thể sử dụng để kết nối với Mamaki Testnet:

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://grpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)
* [https://rpc.mamaki.celestia.counterpoint.software](https://rpc.mamaki.celestia.counterpoint.software)

## Vòi token Mamaki Testnet

> SỬ DỤNG VÒI NÀY SẼ KHÔNG GIÚP BẠN NHẬN BẤT KÌ AIRDROP NÀO HOẶC CÁC ĐỢT PHÂN PHỐI TOKENS CELESTIA MAINNET. TOKENS CELESTIA MAINNET HIỆN KHÔNG TỒN TẠI VÀ HIỆN KHÔNG CÓ BẤT KÌ ĐỢT BÁN CÔNG KHAI HOẶC ĐỢT PHÂN PHỐI TOKEN CELESTIA MAINNET NÀO.

Bạn có thể yêu cầu từ Vòi Mamaki Testnet trên kênh #mamaki-faucet trong Máy chủ Discord của Celestia với lệnh sau:

```text
$request <CELESTIA-ADDRESS>
```

Thay `<CELESTIA-ADDRESS>` bằng địa chỉ `celestia1******` được tạo.

> Lưu ý: Vòi có giới hạn 10 tokens một tuần đối với mỗi địa chỉ ví/Discord ID

## Trình khám phá khối

Có nhiều trình khám phá khối bạn có thể sử dụng cho Mamaki:

* [https://testnet.mintscan.io/celestia-testnet](https://testnet.mintscan.io/celestia-testnet)
* [https://celestia.explorers.guru/](https://celestia.explorers.guru/)
* [https://celestiascan.vercel.app/](https://celestiascan.vercel.app/)

## Thiết lập mạng P2P

Bây giờ chúng ta sẽ thiết lập Mạng P2P bằng cách sao chép repository của mạng:

```sh
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git
```

Để khởi tạo mạng, hãy chọn "node-name", thứ mô tả node của bạn. Thông số --chain-id chúng ta sẽ sử dụng là `mamaki`. Hãy lưu ý rằng điều này có thể sẽ thay đổi nếu một testnet mới được triển khai.

```sh
celestia-appd init "node-name" --chain-id mamaki
```

Sao chép file `genesis.json`. Đối với mamaki chúng ta sẽ sử dụng:

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

Chỉnh seeds và peers:

<!-- markdownlint-disable MD013 -->
```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml
```
<!-- markdownlint-enable MD013 -->

Ghi chú: Bạn có thể tìm thêm peers [tại đây](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt).

## Sync nhanh bằng snapshot

Chạy những câu lệnh sau để sync nhanh bằng snapshot của `mamaki`:

```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```

## Ủy thác cho một validator

Để ủy thác tokens đến địa chỉ `celestiavaloper` của validator, ví dụ, bạn có thể chạy:

```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```

Nếu thành công, bạn sẽ thấy kết quả tương tự như sau:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Bạn có thể kiểm tra xem TX hash đã hoàn thành hay chưa bằng cách sử dụng trình khám phá khối bằng cách nhập ID `txhash` đã được trả về.

## Kết nối Validator

Tiếp tục với hướng dẫn thiết lập Validator, đây là các bước để kết nối validator của bạn với Mamaki:

```sh
MONIKER="your_moniker"
VALIDATOR_WALLET="validator"

celestia-appd tx staking create-validator \
    --amount=1000000utia \
    --pubkey=$(celestia-appd tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=mamaki \
    --commission-rate=0.1 \
    --commission-max-rate=0.2 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1000000 \
    --from=$VALIDATOR_WALLET \
    --keyring-backend=test
```

Bạn sẽ được nhắc xác nhận giao dịch:

```console
xác nhận giao dịch trước khi ký và phát [y / N]: y
```

Nhập ` y ` phải cung cấp đầu ra tương tự như:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Bây giờ bạn có thể thấy validator của mình từ block explorer như [ here ](https://celestia.explorers.guru/)
