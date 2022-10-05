- - -
sidebar_label : Встановлення застосунків Celestia
- - -

# Додаток Celestia
<!-- markdownlint-disable MD013 -->

Цей посібник проведе вас через будівництво додатку Celestia. Це керівництво припускає, що ви виконали кроки з налаштування власного середовища [тут](./environment.md).

## Встановити додаток Celestia

Наведені нижче кроки створять двійковий файл з ім'ям `celestia-appd` у теці`$HOME/go/bin`, який пізніше використовуватиметься для запуску ноди.

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

Щоб перевірити, чи був двійковий файл успішно скомпільований, ви можете запустити його, використовуючи позначку `--help`:

```sh
celestia-appd --help
```

Ви повинні побачити подібний вивід (з корисними прикладами команд):

```text
Start celestia app

Usage:
  celestia-appd [command]

Доступні команди:
  add-genesis-account Додати обліковий запис genesis до genesis.json
  collect-gentxs      Зібрати файл genesis txs і вивести файл genesis.json
  config              Створити або запитати файл конфігурації CLI програми
  debug               Інструмент для допомоги в налагодженні вашої програми
  export              Експорт стану в JSON
  gentx               Створити  genesis tx із самоделегуванням
  help                Довідка про будь-яку команду
  init                Ініціалізувати приватний валідатор, p2p, genesis і файли конфігурації програми
  keys                Керувати ключами вашої програми
  migrate             Перенести Genesis до вказаної цільової версії
  query               Підкоманди запиту
  rollback            відкотити стан tendermint на одну висоту
  rollback            відкотити cosmos-sdk і стан tendermint на одну висоту
  start               Запустити повну ноду
  status              Запитати стан віддаленої ноди
  tendermint          Підкоманди Tendermint
  tx                  Підкоманди транзакцій
  validate-genesis    перевіряє файл genesis у розташуванні за замовчуванням або в розташуванні, переданому як аргумент
  version             Роздрукуйте інформацію про двійкову версію програми

Прапорці:
  -h, --help                довідка для celestia-appd
      --home string        каталог для конфігурації та даних (за замовчуванням "/root/.celestia-app")
      --log_format string   Формат журналювання (json|plain) (за замовчуванням "plain")
      --log_level string    Рівень журналювання (трасування|налагодження|інформація|попередження|помилка|фатальна|паніка) (за замовчуванням "інформація")
      --trace               роздрукувати повне трасування стека для помилок

Використовуйте "celestia-appd [command] --help", щоб отримати додаткові відомості про команду.
```
