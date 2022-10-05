- - -
Cài đặt Celestia App
- - -

# Celestia App
<!-- markdownlint-disable MD013 -->

Hướng dẫn này sẽ hướng dẫn bạn xây dựng Celestia App. Đây hướng dẫn giả định rằng bạn đã hoàn thành các bước trong việc thiết lập môi trường riêng [ tại đây ](./environment.md).

## Cài đặt Celestia App

Các bước dưới đây sẽ tạo một tệp nhị phân có tên ` celestia-appd ` bên trong thư mục ` $ HOME / go / bin ` sẽ được sử dụng sau này để chạy node.

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

Để kiểm tra xem tệp nhị phân đã được biên dịch thành công hay chưa, bạn có thể chạy tệp nhị phân sử dụng cờ ` --help `:

```sh
celestia-appd --help

```

Bạn sẽ thấy một kết quả tương tự (với các lệnh ví dụ hữu ích):

```text
Chạy celestia app
Sử dụng:
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
  validate-genesis    Xác thực tệp genesis tại vị trí mặc định hoặc tại vị trí được truyền dưới dạng đối số  version             In thông tin phiên bản nhị phân của ứng dụng
Cờ:
  -h, --help                Giúp đỡ cho celestia-appd
      --home string         Thư mục cho cấu hình và dữ liệu (mặc định "/root/.celestia-app")
      --log_format string   Định dạng ghi nhật ký(json|plain) (mặc định "plain")
      --log_level string    Mức độ ghi nhật ký(trace|debug|info|warn|error|fatal|panic) (mặc định "info")
      --trace               In ra dấu vết ngăn xếp đầy đủ về lỗi

Sử dụng "celestia-appd [command] --help" để biết thêm thông tin về lệnh.
```
