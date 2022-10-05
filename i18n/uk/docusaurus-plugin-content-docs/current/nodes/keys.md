- - -
sidebar_label : Ключі
- - -

# Використання утиліти cel-key

Inside the celestia-node repository is a utility called `cel-key` that uses the key utility provided by Cosmos-SDK under the hood. The utility can be used to `add`, `delete`, and manage keys for any DA node type `(bridge || full || light)`, or just keys in general.

## Встановлення

Спочатку вам потрібно запустити репозиторій `celestia-node`:

```sh
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

Його можна створити за допомогою однієї з наступних команд:

```sh
# dumps binary in current working directory, accessible via `./cel-key`
make cel-key
```

або

```sh
# installs binary in GOBIN path, accessible via `cel-key`
make install-key
```

Для цілей цього гайду ми будемо використовувати команду `make cel-key`.

## Кроки для генерації ключів **моствої ноди**

Щоб створити ключ для мостової ноди Celestia, виконайте такі команди:

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

Це завантажить ключ <key_name> у каталог мостової ноди.

## Кроки для генерації ключів **повної ноди**

Щоб створити ключ для повної ноди Celestia, виконайте такі команди:

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

Це завантажить ключ <key_name> у каталог повної ноди.

## Кроки для генерації ключів **легкої ноди**

Щоб створти ключ для легкої ноди Celestia, виконайте такі команди:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

Це завантажить ключ <key_name> у каталог легкої ноди.

## Кроки для експорту ключів **легкої ноди**

Ви можете експортувати приватний ключ із локального зв’язку ключів у зашифрованому та ASCII-захищеному форматі.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

Потім ви можете імпортувати свій ключ за допомогою `celestia-appd`:

```sh
celestia-appd keys import <new-key-name> <key-file-location>
```
