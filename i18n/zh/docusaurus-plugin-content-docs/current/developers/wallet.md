- - -
sidebar_label : 创建钱包
- - -

# 钱包

## 创建钱包

首先，创建一个应用程序 CLI 配置文件：

```sh
celestia-appd config keyring-backend test
```

你可以选择任何你想要的钱包名称。 对于我们的示例，我们使用“validator”作为钱包名称：

```sh
celestia-appd keys add validator
```

保存好助记词，因为这是找回钱包的唯一方法，以防丢失！

要检查你所有的钱包，你可以运行：

```sh
celestia-appd keys list
```

## 给钱包充值

For the public celestia address, you can fund the previously created wallet via [Discord](https://discord.gg/celestiacommunity) by sending this message to #arabica-faucet channel:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

请等待确认代币已成功发送的消息。 可以通过运行以下替换了自己钱包地址的命令查看代币是否成功发送到目的钱包：

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
