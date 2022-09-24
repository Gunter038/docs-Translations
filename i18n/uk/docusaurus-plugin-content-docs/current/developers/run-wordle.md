---
sidebar_label: Запустіть ланцюжок Wordle
---

# Запустіть ланцюжок Wordle
<!-- markdownlint-disable MD013 -->

## Створення та запуск Wordle Chain

В одному вікні терміналу виконайте таку команду:

```sh
ignite chain serve 
```

Це скомпілює код блокчейну, який ви щойно написали, а також створить файл genesis та кілька облікових записів для використання. Після того, як журнал покаже щось на зразок наступного журналу у вихідних даних:

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5
📦 Встановлення залежностей...
🛠️ Створення блокчейну...
💿 Ініціалізація програми...
🙂 Створено обліковий запис "alice" з адресою "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" із мнемонікою: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Створено обліковий запис "bob" з адресою "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" із мнемонікою: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Тут команда створила двійковий файл `wordled` та адреси `alice` та `bob`, а також кран і API. Ви впевнені, що хочете вийти з програми з CTRL-C. Причина цього в тому, що ми запускатимемо двійковий файл `wordled` окремо з доданими прапорцями Optimint.

Ви можете почати ланцюжок із конфігурацій optimint, виконавши наступне:

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Будь ласка, зверніть увагу:

> ПРИМІТКА. У наведеній вище команді вам потрібно передати IP-адресу ноди Celestia до `base_url`, що має обліковий запис із токенами Arabica Devnet. Дотримуйтеся посібника з налаштування Celestia Light Node і створення гаманця з тестовими токенами [тут](./node-tutorial.md) у розділі Celestia Node.

Також зверніть увагу:

> ВАЖЛИВО: Крім того, у наведеній вище команді вам потрібно вказати останню висоту блоку в Arabica Devnet для `da_height`. Ви можете знайти останній номер блоку в провіднику [тут](https://explorer.celestia.observer/arabica). Крім того, для прапора `--optimint.namespace_id` ви можете згенерувати випадковий ідентифікатор простору імен за допомогою ігрового майданчика [тут](https://go.dev/play/p/7ltvaj8lhRl)

В іншому вікні виконайте наступне, щоб надіслати Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

> ПРИМІТКА. Ми надсилаємо транзакцію асинхронно, щоб уникнути помилок часу очікування. Завдяки Optimint як заміні Tendermint нам потрібно дочекатися, поки мережа доступності даних Celestia переконається, що блок включено з Wordle, перш ніж переходити до наступного блоку. Наразі в Optimint єдиний агрегатор не просувається вперед зі створенням наступного блоку, доки він намагається надіслати поточний блок до мережі DA. У майбутньому, з вибором лідера, виробництво блоків і логіка синхронізації значно покращуються.

Це попросить вас підтвердити транзакцію з таким повідомленням:

```json
{
  "body":{
    "messages":[
       {
          "@type":"/YazzyYaz.wordle.wordle.MsgSubmitWordle",
          "creator":"cosmos17lk3fgutf00pd5s8zwz5fmefjsdv4wvzyg7d74",
          "word":"giant"
       }
    ],
    "memo":"",
    "timeout_height":"0",
    "extension_options":[
    ],
    "non_critical_extension_options":[
    ]
  },
  "auth_info":{
    "signer_infos":[
    ],
    "fee":{
       "amount":[
       ],
       "gas_limit":"200000",
       "payer":"",
       "granter":""
    }
  },
  "signatures":[
  ]
}
```

Космос-SDK попросить вас підтвердити транзакцію:

```sh
confirm transaction before signing and broadcasting [y/N]:
```

Підтвердити за допомогою Y.

Ви отримаєте відповідь з хешем транзакції, як показано тут:

```sh
code: 19
codespace: sdk
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128
```

Зверніть увагу, це не означає, що транзакція ще була включена в блок. Давайте запитаємо хеш транзакції, щоб перевірити, чи він уже включений у блок, чи є якісь помилки.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

Це має показувати вивід, як наступний результат:

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Протестуйте декілька речей для задоволення:

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

Після підтвердження транзакції надішліть запит `txhash`, так само як ви робили вище. Ви побачите, що у відповіді буде повідомлено про недійсну помилку, оскільки ви надіслали цілі числа.

А тепер спробуйте:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

Після підтвердження транзакції надішліть запит `txhash`, так само як ви робили вище. Ви побачите, що у відповіді буде повідомлено про недійсну помилку, оскільки ви надіслали слово, яке містить понад 5 символів.

Тепер спробуйте надіслати інше слово, хоча одне вже було надіслано

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

Після надсилання транзакцій і підтвердження надішліть запит `txhash`, так само як ви робили вище. Ви отримаєте повідомлення про помилку, що слово на день уже надіслано.

Тепер спробуймо вгадати п'ять літер:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

Після надсилання транзакцій і підтвердження надішліть запит `txhash`, так само як ви робили вище. Якщо ви не вгадали правильне слово, це збільшить кількість вгаданих для облікового запису Боба.

Ми можемо перевірити це, запитавши список:

```sh
wordled q wordle list-guess --output json
```

Це виведе всі об’єкти Guess, надіслані на цю мить, з індексом, який є сьогоднішньою датою та адресою відправника.

Таким чином, ми реалізували базовий приклад Wordle за допомогою Cosmos-SDK і Ignite та Optimint. Прочитайте далі, як розширити кодову базу.

## Розширення в майбутньому

Ви можете розширити кодову базу та покращити цей підручник, переглянувши репозиторій [тут](https://github.com/celestiaorg/wordle).

Є багато способів, як ця кодова база може бути розширена:

1. Можна поліпшити обмін повідомленнями, якщо вгадати правильне слово.
2. Ви можете хешувати слово перед тим, як надсилати його в ланцюжок, гарантуючи, що хешування є локальним, щоб воно не було виявлено через переднє виконання іншими користувачами, які відстежують рядок відкритого тексту, коли він надсилається в ланцюжок.
3. Ви можете покращити інтерфейс терміналу за допомогою приємного інтерфейсу для Wordle. Деякі приклади є [тут](https://github.com/nimblebun/wordle-cli).
4. Ви можете покращити поточну дату, щоб дотримуватися певної часової зони.
5. Ви можете створити бота, який надсилає wordle щодня в певний час.
6. Ви можете створити інтерфейс vue.js за допомогою Ignite, використовуючи приклади репозиторіїв з відкритим кодом [тут](https://github.com/yyx990803/vue-wordle) і [тут](https://github.com/xudafeng/wordle).
