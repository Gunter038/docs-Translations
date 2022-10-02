- - -
sidebar_label : Легка нода
- - -

# Налаштування легкої ноди Celestia

Цей підручник допоможе вам налаштувати легку ноду Celestia, яка дозволить виконувати вибірку доступності даних у мережі доступності даних (DA).

> Для перегляду відеопосібника з налаштування ноди Celestia, натисніть [тут](../developers/light-node-video.md)

## Огляд легких нод

Легкі ноди забезпечують доступність даних. Це найбільш поширений спосіб взаємодії з мережею Celestia.

![light-node](/img/nodes/LightNodes.png)

Легкі ноди мають таку поведінку:

1. Вони слухають ExtendedHeaders, тобто загорнуті «необроблені» заголовки, які сповіщають ноди Celestia про нові заголовки блоків і відповідні метадані DA.
2. Вони виконують вибірку доступних даних (DAS) на прийнятих заголовках

## Вимоги до обладнання

Для роботи легкої ноди рекомендуються такі мінімальні вимоги до обладнання:

* Розмір оперативної пам’яті: 2 GB
* CPU: Єдине ядро
* Диск: 5 ГБ SSD-накопичувач
* Пропускна здатність: 56 Кбіт/с для завантаження/56 Мбіт/с для вивантаження

## Налаштування вашої легкої ноди

Наступний навчальний посібник виконано на машині з дистрибутивом Ubuntu Linux 20.04 (LTS) x64.

### Встановлення залежностей

Спочатку переконайтеся, що бажаєте оновити та покращити ОС:

```sh
# Якщо ви використовуєте менеджер пакетів APT
sudo apt update && sudo apt upgrade -y

# Якщо ви використовуєте менеджер пакетів YUM
sudo yum update
```

Це основні пакети, необхідні для виконання багатьох завдань, таких як завантаження файлів, компіляція та моніторинг ноди:

```sh
# Якщо ви використовуєте менеджер пакетів APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# Якщо ви використовуєте менеджер пакетів YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```

### Інсталювати Golang

Celestia-app та elestia-node написано в [Golang](https://go.dev/), тому нам слід встановити Golang, щоб побудувати та запустити їх.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Тепер нам потрібно додати каталог `/usr/local/go/bin` до `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Щоб перевірити, чи був Go встановлений правильно:

```sh
go version
```

Вихід повинен бути встановленою версією:

```sh
go version go1.18.2 linux/amd64
```

### Встановлення ноди Celestia

Одна річ, яку тут слід зазначити, це можливість вирішити, яку версію celestia-node ви хочете скомпілювати. Mamaki Testnet вимагає v0.3.0-rc2, а Arabica Devnet вимагає v0.3.0.

У наступних розділах описано, як її встановити для двох мереж.

#### Встановлення Arabica Devnet

Встановіть двійковий файл celestia-node, виконавши такі команди:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
make cel-key
```

Verify that the binary is working and check the version with the celestia version command:

```sh
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

#### Встановлення тестової мережі Mamaki

Встановіть двійковий файл celestia-node, виконавши такі команди:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
make cel-key
```

Переконайтеся, що двійковий файл працює, і перевірте версію за допомогою команди celestia version:

```sh
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Ініціалізація легкої ноди

Запустіть таку команду:

```sh
celestia light init
```

Ви повинні побачити такий вивід:

<!-- markdownlint-disable MD013 -->
```output
$ celestia light init
2022-07-18T02:22:09.449Z INFO node node/init.go:26 Initializing Light Node Store over '/home/ec2-user/.celestia-light'
2022-07-18T02:22:09.449Z INFO node node/init.go:62 Saving config {"path": "/home/ec2-user/.celestia-light/config.toml"}
2022-07-18T02:22:09.449Z INFO node node/init.go:67 Node Store initialized
```
<!-- markdownlint-enable MD013 -->

### Запуск легкої ноди

Запустіть легку ноду з підключенням до кінцевої точки gRPC ноди перевірки (яка зазвичай доступна на порту 9090):

> ПРИМІТКА. Щоб отримати доступ до можливості отримати/надсилати інформацію, пов’язану зі станом, наприклад можливість надсилати транзакції PayForData або запитувати баланс рахунку ноди, кінцеву точку gRPC ноди валідатора (основного) потрібно передати відповідно до вказівок нижче.

Для портів:

> ПРИМІТКА: `--core.grpc.port` за замовчуванням має значення 9090, тому, якщо ви не вкажете іншого в командному рядку, за замовчуванням використовуватиметься цей порт. Можна використовувати прапорець, щоб вказати за бажанням інший порт.

```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port>
```

#### Arabica Setup

Якщо вам потрібен список кінцевих точок RPC для підключення, ви можете ознайомитися зі списком [тут](./arabica-devnet.md#rpc-endpoints)

Наприклад, ваша команда може виглядати приблизно так:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://limani.celestia-devops.dev --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

#### Mamaki Setup

If you need a list of RPC endpoints to connect to, you can check from the list [here](./mamaki-testnet.md#rpc-endpoints)

For example, your command might look something like this:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip https://rpc-mamaki.pops.one --core.grpc.port 9090
```
<!-- markdownlint-enable MD013 -->

### Ключі та гаманці

Ви можете створити ключ для вашої ноди, запустивши таку команду:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <key_name>
```
<!-- markdownlint-enable MD013 -->

Після запуску легкої ноди для вас буде згенеровано ключ гаманця. Вам потрібно буде поповнити цю адресу за допомогою токенів Testnet для оплати транзакцій PayForData.

Ви можете знайти адресу, запустивши таку команду в каталозі `сelestia-node`:

```sh
./cel-key list --node.type light --keyring-backend test
```

У вас є дві мережі для отримання токенів testnet:

* [Arabica](./arabica-devnet.md#arabica-devnet-faucet)
* [Mamaki](./mamaki-testnet.md#mamaki-testnet-faucet)

> ПРИМІТКА. Якщо ви використовуєте для свого суверенного ролапа легку ноду, наполегливо рекомендуємо запитувати токени Arabica devnet, оскільки в Arabica є останні зміни, які можна використовувати для тестування для розробки вашого суверенного ролапа. Ви все ще можете використовувати Mamaki Testnet, але він переважно використовується для операцій валідатора.

Ви можете попросити кошти на адресу гаманця за допомогою такої команди в Discord:

```console
$request <Wallet-Address>
```

Де `<Wallet-Address>` – це адреса `celestia1******`, що була згенерована під час створення гаманця.

### Необов’язково: запустити легку ноду зі спеціальним ключем

Щоб запустити легку ноду за допомогою користувацького ключа:

1. Спеціальний ключ має існувати в каталозі вузла celestia light за правильним шляхом (за замовчуванням: `~/.celestia-light/keys/keyring-test`)
2. Ім'я користувацького ключа має бути передано під час `запуску`, наприклад:

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --keyring.accname <name_of_custom_key>
```
<!-- markdownlint-enable MD013 -->

### Необов'язково: запустити легку ноду з SystemD

Дотримуйтеся підручника з налаштування легкої ноди як фонового процесу за допомогою SystemD [тут](./systemd.md#celestia-light-node).

## Вибірка доступності даних (DAS)

Коли вашу легку ноду запущено, ви можете [тут](../developers/node-tutorial.md) переглянути цей підручник щодо надсилання транзакцій `PayForData`.
