- - -
sidebar_label : SystemD
- - -

# Thiết lập node của bạn làm quy trình nền với SystemD

SystemD là một dịch vụ daemon hữu ích để chạy các ứng dụng dưới dạng quy trình nền.

## Các nodes đồng thuận

Nếu bạn đang chạy validator hoặc node đồng thuận đầy đủ, đây là các bước để thiết lập ` celestia-appd ` làm quy trình nền.

### Khởi động ứng dụng celestia với SystemD

SystemD là một dịch vụ daemon hữu ích để chạy các ứng dụng dưới dạng quy trình nền.

Tạo tệp hệ thống Celestia-App:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-appd.service
[Unit]
Description=celestia-appd Cosmos daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia-appd start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
EOF
```

Nếu tệp được tạo thành công, bạn sẽ có thể xem nội dung của nó:

```sh
cat /etc/systemd/system/celestia-appd.service
```

Bật và khởi động daemon celestia-appd:

```sh
systemctl enable celestia-appd
systemctl start celestia-appd
```

Kiểm tra xem daemon đã được khởi động đúng cách chưa:

```sh
systemctl status celestia-appd
```

Kiểm tra nhật ký daemon trong thực tế:

```sh
journalctl -u celestia-appd.service -f
```

Để kiểm tra xem node của bạn có được đồng bộ hóa hay không trước khi tiếp tục:

```sh
curl -s localhost:26657/status | jq .result | jq .sync_info
```

Đảm bảo rằng bạn có ` "catch_up": false `, nếu không, hãy để nó chạy cho đến khi nó được đồng bộ.

## Các node về tính khả dụng của dữ liệu

### Node lưu trữ đầy đủ của Celestia

Tạo tệp hệ thống nút lưu trữ đầy đủ Celestia:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-full.service
[Unit]
Description=celestia-full Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia full start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Nếu tệp được tạo thành công, bạn sẽ có thể xem nội dung của nó:

```sh
cat /etc/systemd/system/celestia-full.service
```

Kích hoạt và khởi động celestia-full daemon:

```sh
systemctl enable celestia-full
systemctl start celestia-full && journalctl -u \
celestia-full.service -f
```

Bạn sẽ thấy nhật ký từ quá trình đồng bộ hóa node lưu trữ đầy đủ.

### Celestia bridge node

Tạo tệp hệ thống Celestia Bridge:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-bridge.service
[Unit]
Description=celestia-bridge Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia bridge start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Nếu file được tạo thành công, bạn sẽ thấy nội dung nó như bên dưới:

```sh
cat /etc/systemd/system/celestia-bridge.service
```

Kích hoạt và khởi động trình chạy nền cho celestia-bridge:

```sh
systemctl enable celestia-bridge
systemctl start celestia-bridge && journalctl -u \
celestia-bridge.service -f
```

Bây giờ, node Bridge Celestia sẽ bắt đầu đồng bộ hóa các headers và lưu trữ các khối từ ứng dụng Celestia.

> Lưu ý: Khi bắt đầu, chúng ta có thể nhìn thấy `multiaddress` từ Node Bridge của Celestia. Điều này **cần thiết cho việc kết nối và giao tiếp của các Light node trong tương lai** với các Node Bridge của Celestia

Ví dụ:

```sh
NODE_IP=<ip-address>
/ip4/$NODE_IP/tcp/2121/p2p/12D3KooWD5wCBJXKQuDjhXFjTFMrZoysGVLtVht5hMoVbSLCbV22
```

Bạn sẽ nhìn thấy logs từ quá trình đồng bộ hóa của node bridge.

### Celestia light node

Khởi động Light Node với chương trình daemon chạy nền

<!-- markdownlint-disable MD013 -->
```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
[Unit]
Description=celestia-lightd Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia light start --core.ip <ip-address> --core.grpc.port <port>
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```
<!-- markdownlint-enable MD013 -->

Nếu file được tạo thành công bạn sẽ nhìn thấy nội dung như sau:

```sh
cat /etc/systemd/system/celestia-lightd.service
```

Cho phép và khởi động daemon celestia-lightd:

```sh
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

Kiểm tra xem liệu daemon đã được khởi chạy đúng:

```sh
systemctl status celestia-lightd
```

Kiểm tra logs của daemon đối với thời gian thực:

```sh
journalctl -u celestia-lightd.service -f
```

Bây giờ, Light node Celestia sẽ bắt đầu đồng bộ hóa headers. Sau khi đồng bộ hóa thành công, Light Node sẽ thực hiện Lấy mẫu Data Availability (DAS) từ Node Bridge.
