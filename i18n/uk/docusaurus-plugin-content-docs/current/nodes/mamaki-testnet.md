- - -
sidebar_label : Mamaki Тестнет
- - -

# Тестова мережа Mamaki

![mamaki-testnet](/img/mamaki.png)

Ця інструкція містить відповідні розділи для підключення до тестової мережі Mamaki, в залежності від типу запущеної вами ноди. Mamaki Testnet розроблено, щоб допомогти валідаторам перевірити свою інфраструктуру та програмне забезпечення нод за допомогою тестової мережі. Розробникам рекомендується розгортати свої суверенні зведені пакети на Mamaki, але ми також рекомендуємо для цього [Arabica Devnet](./arabica-devnet.md), оскільки він призначений для розробницьких цілей.

Mamaki - це важлива стадія в Celestia, що дозволяє всім охочим протестувати основні функціональні можливості мережі. Прочитайте анонс [тут](https://blog.celestia.org/celestia-testnet-introduces-alpha-data-availability-api/).

Ваш найкращий підхід до участі — це спочатку визначити, яку ноду ви хочете запустити. Посібники з настройки кожної ноди посилатимутья на відповідну мережу, щоб показати вам, як до них підключитися.

У вас є список варіантів типу нод, які ви можете запустити, щоб взяти участь у Mamaki:

Консенсус:

* [Нода Валідатора](./validator-node.md)
* [Вузол консенсусу](./consensus-full-node.md)

Доступність даних:

* [Мостова Нода](./bridge-node.md)
* [Нода Повного Зберігання](./full-storage-node.md)
* [Спрощена Нода](./light-node.md)

Виберіть тип ноди, яку ви хочете запустити, і дотримуйтесь інструкцій на кожній відповідній сторінці. Щоразу, коли вас попросять вибрати тип мережі, до якої ви хочете підключитися в цих посібниках, виберіть `Mamaki`, щоб переглянути правильні інструкції на цій сторінці щодо підключення до Mamaki.

## Кінцеві точки RPC

Це список кінцевих точок RPC, які ви можете використати для підключення до сервера Mamaki Testnet:

* [https://rpc-mamaki.pops.one](https://rpc-mamaki.pops.one)
* [https://rpc-1.celestia.nodes.guru](https://rpc-1.celestia.nodes.guru)
* [https://grpc-1.celestia.nodes.guru:10790](https://grpc-1.celestia.nodes.guru:10790)
* [https://celestia-testnet-rpc.polkachu.com/](https://celestia-testnet-rpc.polkachu.com/)
* [https://rpc.celestia.testnet.run](https://rpc.celestia.testnet.run/)
* [https://rpc.mamaki.celestia.counterpoint.software](https://rpc.mamaki.celestia.counterpoint.software)

## Кран тестової мережі Mamaki

> ВИКОРИСТАННЯ ЦЬОГО КРАНУ НЕ ДАЄ ВАМ ПРАВА НА ЖОДЕН AIRDROP АБО ІНШЕ РОЗПОДІЛ ТОКЕНІВ ОСНОВНОЇ МЕРЕЖІ CELESTIA. ТОКЕНІВ ОСНОВНОЇ МЕРЕЖІ CELESTIA НАРАЗІ НЕ ІСНУЄ, І НЕМАЄ ПУБЛІЧНИХ ПРОДАЖІВ ЧИ ІНШИХ ПУБЛІЧНИХ РОЗПОВСЮДЖЕНЬ БУДЬ-ЯКИХ ТОКЕНІВ ОСНОВНОЇ МЕРЕЖІ CELESTIA.

Ви можете надіслати запит у кран Mamaki Testnet на каналі #mamaki-faucet на сервері Discord Celestia за допомогою такої команди:

```text
$request <Celestia-Address>
```

Де `<CELESTIA-ADDRESS>` є `celestia1******` створеною адресою гаманця.

> Примітка: Faucet має обмеження на 10 токенів в тиждень на адресу/Discord ID

## Оглядачі

Є кілька оглядачів, які ви можете використовувати для Mamaki:

* [https://testnet.mintscan.io/celestia-testnet](https://testnet.mintscan.io/celestia-testnet)
* [https://celestia.explorers.guru/](https://celestia.explorers.guru/)
* [https://celestiascan.vercel.app/](https://celestiascan.vercel.app/)

## Налаштування мережі P2P

Тепер ми налаштуємо мережі P2P шляхом клонування мереж репозиторію:

```sh
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git
```

Щоб ініціалізувати мережу, виберіть "node-name", що описує вашу ноду. Параметр --chain-id, який ми тут використовуємо, це `mamaki`. Майте на увазі, що --chain-id може змінитися, якщо ми розгорнемо нову тестову мережу.

```sh
celestia-appd init "node-name" --chain-id mamaki
```

Скопіюйте `genesis.json` файл. Для мамамакі ми використовуємо:

```sh
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config
```

Встановіть сіди та піри:

<!-- markdownlint-disable MD013 -->
```sh
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')
echo $BOOTSTRAP_PEERS
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml
```
<!-- markdownlint-enable MD013 -->

Примітка: Ви можете знайти більше пірів [тут](https://github.com/celestiaorg/networks/blob/master/mamaki/peers.txt).

## Швидка синхронізація використовуючи снапшот

Виконайте наступну команду для швидкої синхронізації зі снапшоту для `mamaki`:

```sh
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/
```

## Делегуйте валідатору

Щоб делегувати токени валідатору `celestiavaloper`, як приклад, ви можете виконати:

```sh
celestia-appd tx staking delegate \
    celestiavaloper1q3v5cugc8cdpud87u4zwy0a74uxkk6u43cv6hd 1000000utia \
    --from=$VALIDATOR_WALLET --chain-id=mamaki
```

У разі успіху ви повинні побачити подібний результат:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Ви можете перевірити, чи пройшов хеш транзакції, використовуючи провідник блокчейну, ввівши айді транзакції, яку ви отримали.

## Підключіть валідатора

Продовжуючи посібник, ось кроки для підключення вашого валідатора до Mamaki:

```sh
MONIKER="your_moniker"
VALIDATOR_WALLET="validator"

celestia-appd tx staking create-validator \
    --amount=1000000utia \
    --pubkey=$(celestia-appd tendermint show-validator) \
    --moniker=$MONIKER \
    --chain-id=mamaki \
    --commission-rate=0.1 \
    --commission-max-rate=0.2 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1000000 \
    --from=$VALIDATOR_WALLET \
    --keyring-backend=test
```

Вам буде запропоновано підтвердити транзакцію:

```console
confirm transaction before signing and broadcasting [y/N]: y
```

Введення `y` має дати результат, подібний до:

```console
code: 0
codespace: ""
data: ""
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: <tx-hash>
```

Тепер ви можете бачити свого валідатора у провіднику блокчейну, наприклад [тут](https://celestia.explorers.guru/)
