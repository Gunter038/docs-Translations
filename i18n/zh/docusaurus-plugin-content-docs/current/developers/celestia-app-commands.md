- - -
sidebar_label: 有用的 CLI 命令
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

`keyring-backend`配置存储密钥的keyring的后台实现。

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
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

示例：

```sh
celestia-appd keys import amanda ./keyfile.txt
```

用加密格式和 ASCII-armored 格式从本地密钥中导出私钥：

```sh
celestia-appd keys export <KEY_NAME>

# you will then be prompted to set a password for the encrypted private key:
Enter passphrase to encrypt the exported key:
```

在您设置密码后，您的加密密钥将会显示。

## 查询子命令

用法：

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

查看所有选项

```sh
celestia-appd q --help
```

## 代币管理

获取代币余额：

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

示例：

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

将代币从钱包转移到另一个钱包：

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

示例：

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

查看选项:

```sh
celestia-appd tx bank send --help
```

## 统治

您可以投票在公司管理使用以下命令：

```sh
celestia appd tx gov投票<proposal id><yes or no>--来自<wallet>--链id<chain-id>
```

## 索赔验证者奖励

你可以索赔验证者奖励用以下命令：

```sh
celestia-appd tx distribution withdraw-rewards <validator valoper>\
    --commission --from=<validator wallet> --chain-id <chain-id> --gas auto -y
```

## 委派&amp；取消委派令牌

您可以`delegate` 将您的令牌委托给验证者 使用以下命令：

```sh
celestia-appd tx staking delegate <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

您可以将令牌取消委派给验证者 使用`unbond`命令：

```sh
celestia-appd tx staking unbond <validator valoper> <amount>\
    --from <wallet> --chain-id <chain-id>
```

## 解禁验证者

您可以解除监禁验证者使用以下命令：

```sh
celestia-appd tx slashing unjail --from <validator wallet>\
    --chain-id <chain-id> --gas auto -y
```

## 如何使用SystemD导出日志

如果正在运行，则可以导出日志 使用以下命令的SystemD服务：

```sh
sudo journalctl -u <your systemd service> -S yesterday > node_logs.txt
sudo journalctl -u <your systemd service> -S today > node_logs.txt
#此命令输出最后100万行！
sudo journalctl -u <your systemd service> -n 1000000 > node_logs.txt
```
