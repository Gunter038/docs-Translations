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
git checkout tags/v0.3.0
make install
```

Переконайтеся, що двійковий файл працює, і перевірте версію за допомогою команди `celestia version`:

```console
$ celestia version
Semantic version: v0.3.0
Commit: 00d80c423b2bfacec22a253ce6af3a534a1be3a7
```
