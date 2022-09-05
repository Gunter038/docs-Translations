- - -
sidebar_label: Tạo Celestia Testnet
- - -

# Hướng dẫn cài đặt mạng ứng dụng Celestia

Hướng dẫn này giúp khởi tạo một mạng thử nghiệm mới và thực hiện các bước chính xác như vậy với Celestia-App. Bạn chỉ nên làm theo hướng dẫn này, nếu bạn muốn thử nghiệm với Celestia Testnetwork riêng hoặc nếu bạn muốn thử nghiệm các tính năng mới nhằm xây dựng như là nhà phát triển cốt lõi.

## Các yêu cầu phần cứng

Bạn có thể làm theo các yêu cầu về phần cứng [ tại đây ](../nodes/validator-node.md#hardware-requirements).

## Thiết lập các phần phụ thuộc

Bạn có thể thiết lập các phần phụ thuộc bằng cách làm theo hướng dẫn [ tại đây ](./environment.md).

## Cài đặt ứng dụng Celestia

Bạn có thể cài đặt Ứng dụng Celestia bằng cách làm theo hướng dẫn [ tại đây ](./celestia-app.md).

## Spin Up A Celestia Testnet

Nếu bạn muốn tạo một testnet nhanh với bạn bè của mình, bạn có thể làm theo các bước sau. Unless otherwise noted, every step must be done by everyone who wants to participate in this testnet.

### Optional: Reset Working Directory

If you have already initialized a working directory for `celestia-appd` in the past, you must clean up before reinitializing a new directory. You can do so by running the following command:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Initialize A Working Directory

Run the following command:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* The value we will use for `$VALIDATOR_NAME` is `validator1` but you should choose your own node name.
* The value we will use for `$CHAIN_ID` is `testnet`. The `$CHAIN_ID` must remain the same for everyone participating in this network.

### Create A New Key

Next, run the following command:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

This will create a new key, with a name of your choosing. Save the output of this command somewhere; you'll need the address generated here later. Here, we set the value of our key `$KEY_NAME` to `validator` for demonstration.

### Add Genesis Account KeyName

Run the following command:

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Here `$VALIDATOR_NAME` is the same key name as before; and `$AMOUNT` is something like `10000000000000000000000000utia`.

### Optional: Adding Other Validators

If other participants in your testnet also want to be validators, repeat the command above with the specific amount for their public keys.

Once all the validators are added, the `genesis.json` file is created. You need to share it with all other validators in your testnet in order for everyone to proceed with the following step.

You can find the `genesis.json` at `$HOME/.celestia-app/config/genesis.json`

### Create the Genesis Transaction For New Chain

Run the following command:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

This will create the genesis transaction for your new chain. Here `$STAKING_AMOUNT` should be at least `1000000000utia`. If you provide too much or too little, you will encounter an error when starting your node.

You will find the generated gentx JSON file inside `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Note: If you have other validators in your network, they need to also run the above command with the `genesis.json` file you shared with them in the previous step.

### Creating the Genesis JSON File

Once all participants have submitted their gentx JSON files to you, you will pull all those gentx files inside the following directory: `$HOME/.celestia-appd/config/gentx` and use them to create the final `genesis.json` file.

Once you added the gentx files of all the particpants, run the following command:

```sh
celestia-appd collect-gentxs
```

This command will look for the gentx files in this repo which should be moved to the following directory `$HOME/.celestia-app/config/gentx`.

It will update the `genesis.json` file after in this location `$HOME/.celestia-app/config/genesis.json` which now includes the gentx of other participants.

You should then share this final `genesis.json` file with all the other particpants who must add it to their `$HOME/.celestia-app/config` directory.

Everyone must ensure that they replace their existing `genesis.json` file with this new one created.

### Modify Your Config File

Open the following file `$HOME/.celestia-app/config/config.toml` to modify it.

Inside the file, add the other participants by modifying the following line to include other participants as persistent peers:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

You can find `validator_address` by running the following command:

```sh
celestia-appd tendermint show-node-id
```

The output will be the hex-encoded `validator_address`. The default `port` is 26656.

### Instantiate the Network

You can start your node by running the following command:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
