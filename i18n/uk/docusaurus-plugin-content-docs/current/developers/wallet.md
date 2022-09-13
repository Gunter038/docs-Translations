- - -
sidebar_label : Створення Гаманця
- - -

# Гаманець

## Створення гаманця

Спочатку створіть файл конфігурації CLI програми:

```sh
celestia-appd config keyring-backend test
```

Ви можете обрати будь-яку назву гаманця. У нашому прикладі ми використали «validator» як назву гаманця:

```sh
celestia-appd keys add validator
```

Save the mnemonic output as this is the only way to recover your validator wallet in case you lose it!

To check all your wallets you can run:

```sh
celestia-appd keys list
```

## Fund a Wallet

For the public celestia address, you can fund the previously created wallet via [Discord](https://discord.gg/celestiacommunity) by sending this message to #faucet channel:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Wait to see if you get a confirmation that the tokens have been successfully sent. To check if tokens have arrived successfully to the destination wallet run the command below replacing the public address with your own:

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
