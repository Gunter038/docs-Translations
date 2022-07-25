- - -
sidebar_label : Helpful CLI commands
- - -

# Helpful CLI commands

View all options:

```console
CLI 命令
$ celestia-appd --help  帮助
Start celestia app  
启动 celestia 应用程序
Usage:使用方法。
    celestia-appd [command]   
    celestia-appd [command] (命令)
Available Commands:   可用的命令。
  add-genesis-account Add a genesis account to genesis.json  
  在genesis.json中添加一个genesis账户
  collect-gentxs      Collect genesis txs and output a genesis.json file
  收集 genesis txs 并输出一个 genesis.json 文件
  config              Create or query an application CLI configuration file
  config 创建或查询一个应用CLI配置文件
  debug               Tool for helping with debugging your application
  debug 用于帮助调试你的应用程序的工具
  export              Export state to JSON
  将状态导出为JSON格式
  gentx               Generate a genesis tx carrying a self delegation
  生成一个携带自我授权的创世纪tx
  help                Help about any command
  关于任何命令的帮助
  init                Initialize private validator, p2p, genesis,
  初始化私有验证器、p2p、genesis。 
  and application configuration files
  和应用程序的配置文件
  keys                Manage your application's keys
  管理你的应用程序的密钥
  migrate             Migrate genesis to a specified target version
  将genesis迁移到一个指定的目标版本
  query               Querying subcommands
  查询子命令
  rollback            rollback tendermint state by one height
  回滚 tendermint 状态的一个高度
  rollback            rollback cosmos-sdk and tendermint state by one height
  滚回 cosmos-SDK 和 tendermint 状态的高度。
  start               Run the full node
  运行整个节点
  status              Query remote node for status
  查询远程节点的状态
  tendermint          Tendermint subcommands
  tendermint Tendermint子命令
  tx                  Transactions subcommands
  tx 交易子命令
  validate-genesis    validates the genesis file at the default
  validate-genesis 验证默认位置的genesis文件，或者验证作为参数传递的位置的genesis文件。 
  location or at the location passed as an arg
  arg作为参数传递的位置
  version             Print the application binary version information
  版本 打印应用程序的二进制版本信息
```

## Creating a wallet

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` configures the keyring's backend, where the keys are stored.

Options are: `os|file|kwallet|pass|test|memory`.

## Key management

```sh
# listing keys
celestia-appd keys list

# adding keys
celestia-appd keys add <KEY_NAME>

# deleting keys
celestia-appd keys delete <KEY_NAME>

# renaming keys
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
