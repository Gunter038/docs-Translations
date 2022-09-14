- - -
sidebar_label : Keys
- - -

# Sử dụng tiện ích cel-key

Trong kho lưu trữ node của celestia là một tiện ích được gọi ` cel-key ` sử dụng tiện ích chính do Cosmos-SDK cung cấp. Tiện ích có thể được sử dụng để ` add `, ` delete ` và quản lý khóa cho bất kỳ node DA nào, gõ ` (bridge || full || light) `, hoặc chỉ các phím nói chung.

## Cài đặt

Đầu tiên, bạn cần phải kéo xuống kho lưu trữ ` celestia-node `:

```sh
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

Nó có thể được tạo bằng một trong các lệnh sau:

```sh
# dumps binary in current working directory, accessible via `./cel-key`
make cel-key
```

hay

```sh
# installs binary in GOBIN path, accessible via `cel-key`
make install-key
```

Với mục đích của hướng dẫn này, chúng ta sẽ sử dụng câu lệnh `make cel-key`.

## Các bước tạo keys cho **bridge** node

Để tạo một key cho node bridge của Celestia, thực hiện các bước sau:

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

Việc này sẽ tải key <key_name> vào trong thư mục của node bridge.

## Các bước tạo keys cho **full** node

Để tạo key cho một full node Celestia, thực hiện các bước sau:

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

Việc này sẽ tải key <key_name> vào trong thư mục của full node.

## Các bước tạo keys cho **light** node

Để tạo key cho một light node Celestia, thực hiện các bước sau:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

Việc này sẽ tải key <key_name> vào trong thư mục của light node.

## Các bước xuất keys cho **light** node

Bạn có thể xuất một private key từ khóa cục bộ ở dạng mã hóa và định dạng ASCII-armored.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

Sau đó bạn có thể nhập key của bạn với `ứng dụng celestia`:

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
