- - -
sidebar_labar: 有用的 CLI 命令
- - -

# 有用的 CLI 命令

查看所有选项

```console
$ celestia-appd --help
Start celestia app

Usage:
  celestia-appd [command]

Available Commands:
  add-genesis-account Add a genesis account to genesis.json
  collect-gentxs      Collect genesis txs and output a genesis.json file
  config              Create or query an application CLI configuration file
  debug               Tool for helping with debugging your application
  export              Export state to JSON
  gentx               Generate a genesis tx carrying a self delegation
  help                Help about any command
  init                Initialize private validator, p2p, genesis, 
  and application configuration files
  keys                Manage your application's keys
  migrate             Migrate genesis to a specified target version
  query               Querying subcommands
  rollback            rollback tendermint state by one height
  rollback            rollback cosmos-sdk and tendermint state by one height
  start               Run the full node
  status              Query remote node for status
  tendermint          Tendermint subcommands
  tx                  Transactions subcommands
  validate-genesis    validates the genesis file at the default 
  location or at the location passed as an arg
  version             Print the application binary version information
```

## 创建钱包

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` 配置存储密钥的密钥环的后端。

选项是:`os|file|kwallet|pass|test|memory`。

## 密钥管理

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

### 导入和导出

导入加密的 ASCII-armored 私钥到本地密钥。

```sh
celestia-appd 密钥导入 <KEY_NAME> <KEY_FILE>
```

示例：

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
