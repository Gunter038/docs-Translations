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

复制您的celestia 地址，然后将地址发送到官方的Discord #faucet频道，为之前创建的钱包充值代币。

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

请等待确认代币已成功发送的消息。 要检查代币是否已成功到达您的钱包，运行下面的命令。注意替换为您自己的公共地址:

```sh
celestia-appd q bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
