- - -
sidebar_label : Validator Node
- - -

# Налаштування ноди Celestia Validator

Валідатор ноди дозволяють брати участь у консенсусі в мережі Celestia.

## Системні вимоги

Для роботи ноди Validator рекомендуються наступні мінімальні системі вимоги до апаратного забезпечення ноди Validator:

* Оперативна пам'ять: 8 ГБ  RAM
* ПРОЦЕСОР (CPU): чотирьохядерний
* Диск: 250 ГБ SSD-накопичувач
* Пропускна здатність: 1 Гбіт/с на завантаження/100 Мбіт/с на вивантаження

## Налаштування ноди validator

Наступний навчальний посібник виконано на машині з дистрибутивом Ubuntu Linux 20.04 (LTS) x64.

### Налаштування залежностей

Дотримуйтесь інструкцій щодо встановлення залежностей [тут](../developers/environment.md).

## Розгортання celestia-app

У цьому розділі описано частину 1 налаштування ноди Celestia Validator: запуск демона Celestia App з внутрішньою нодою Celestia Core.

> Примітка: Переконайтеся, що у вас є принаймні 100+ Гб вільного місця для безпечного встановлення та запуску ноди Validator.

### Встановлення celestia-app

Інструкцію по встановленню програми Celestia App можна знайти [тут](../developers/celestia-app.md).

### Налаштування P2P мереж

У цьому розділі інструкції виберіть мережу, до якої ви хочете підключитися:

* [Mamaki](./mamaki-testnet.md#setup-p2p-network)

Після цього можна приступати до освоєння решти матеріалу інструкції.

### Налаштувати pruning

Для зменшення використання дискового простору ми рекомендуємо налаштувати pruning за допомогою наведених нижче конфігурацій.  За бажанням ви можете змінити їх на власні конфігурації pruning:

```sh
PRUNING="custom"
PRUNING_KEEP_RECENT="100"
PRUNING_INTERVAL="10"

sed -i -e "s/^pruning *=.*/pruning = \"$PRUNING\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \
\"$PRUNING_KEEP_RECENT\"/" $HOME/.celestia-app/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \
\"$PRUNING_INTERVAL\"/" $HOME/.celestia-app/config/app.toml
```

### Конфігурація validator ноди

```sh
sed -i.bak -e "s/^mode *=.*/mode = \"validator\"/" $HOME/.celestia-app/config/config.toml
```

### Перезавантаження мережі

Це призведе до видалення всіх папок з даними, щоб ми могли почати з чистого аркуша:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Додатково: швидка синхронізація зі snapshot

Синхронізація з Genesis може зайняти деякий час, залежно від вашого обладнання. Використовуючи цей метод, ви можете дуже швидко синхронізувати свою ноду Celestia, завантаживши останній снепшот блокчейну. Якщо ви хочете синхронізуватися з Genesis, то можете пропустити цю частину.

Якщо ви хочете скористатися снепшотом, визначте мережу, з якою ви хочете синхронізуватися, зі списку нижче:

* [Mamaki](./mamaki-testnet.md#quick-sync-with-snapshot)

### Запустіть celestia-app за допомогою SystemD

Дотримуйтеся [цієї](./systemd.md#start-the-celestia-app-with-systemd) інструкції з налаштування додатку Celestia як фонового процесу за допомогою SystemD.

### Гаманець

З інструкцією по створенню гаманця можна ознайомитися [here](../developers/wallet.md).

### Делегувати стейк валідатору

Створіть змінну середовища для цієї адреси:

```sh
VALIDATOR_WALLET=<validator-address>
```

Якщо ви хочете делегувати більшу частку будь-якому валідатору, в тому числі і своєму власному, вам знадобиться `celesvaloper` адреса відповідного валідатора. Ви можете перевірити його за допомогою вищезгаданого провідника блоків або запустити команду нижче, щоб отримати `celesvaloper` вашого персонального гаманця валідатора, якщо ви хочете делегувати йому більше токенів:

```sh
celestia-appd keys show $VALIDATOR_WALLET --bech val -a
```

Після введення парольної фрази гаманця ви побачите схожий результат:

```sh
Enter keyring passphrase:
celesvaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd
```

Далі виберіть мережу, в яку ви хочете делегувати валідатору:

* [Розгортання celestia-ноди](./mamaki-testnet.md#delegate-to-a-validator)

## Розгортання celestia-ноди

У цьому розділі описано частину 2 налаштування ноди Celestia Validator: запуск демона ноди Celestia Bridge.

### Встановлення celestia-ноди

Ви можете ознайомитися з інструкцією по встановленню Celestia Ноди [here](../developers/celestia-node.md)

### Ініціалізація bridge ноди

Виконайте наступне:

```sh
celestia bridge init --core.remote tcp://<ip:port of celestia-app> \
  --core.grpc http://<ip:port>
```

Якщо вам потрібен список кінцевих точок RPC для підключення, ви можете ознайомитися зі списком [here](./mamaki-testnet.md#rpc-endpoints)

### Run the bridge node

Run the following:

```sh
celestia bridge start
```

### Optional: start the bridge node with SystemD

Follow the tutorial on setting up the bridge node as a background process with SystemD [here](./systemd.md#celestia-bridge-node).

You have successfully set up a bridge node that is syncing with the network.

## Run a validator node

After completing all the necessary steps, you are now ready to run a validator! In order to create your validator on-chain, follow the instructions below. Keep in mind that these steps are necessary ONLY if you want to participate in the consensus.

Pick a `moniker` name of your choice! This is the validator name that will show up on public dashboards and explorers. `VALIDATOR_WALLET` must be the same you defined previously. Parameter `--min-self-delegation=1000000` defines the amount of tokens that are self delegated from your validator wallet.

Now, connect to the network of your choice.

You have the following option of connecting to list of networks shown below:

* [Mamaki](./mamaki-testnet.md#connect-validator)

Complete the instructions in the respective network you want to validate in to complete the validator setup process.
