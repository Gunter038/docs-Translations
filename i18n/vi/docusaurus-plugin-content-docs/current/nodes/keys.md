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

For the purpose of this guide, we will use the `make cel-key` command.

## Steps for generating **bridge** node keys

To generate a key for a Celestia bridge node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

This will load the key <key_name> into the directory of the bridge node.

## Steps for generating **full** node keys

To generate a key for a Celestia full node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

This will load the key <key_name> into the directory of the full node.

## Steps for generating **light** node keys

To generate a key for a Celestia light node, do the following:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

This will load the key <key_name> into the directory of the light node.

## Steps for exporting **light** node keys

You can export a private key from the local keyring in encrypted and ASCII-armored format.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

You can then import your key with `celestia-appd`:

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
