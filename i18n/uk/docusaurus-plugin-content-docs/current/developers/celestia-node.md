- - -
sidebar_label : Встановлення ноди Celestia
- - -

# Нода Celestia

У цьому посібнику розповідається про створення та встановлення celestia-node. Цей підручник передбачає, що ви виконали кроки з налаштування середовища розробки [тут](./environment.md).

## Установка ноди Celestia

Встановіть двійковий файл celestia-node, виконавши такі команди:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Переконайтеся, що двійковий файл працює, і перевірте версію за допомогою команди `celestia version`:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```
