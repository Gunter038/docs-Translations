- - -
sidebar_label : Встановлення ноди Celestia
- - -

# Нода Celestia

У цьому посібнику розповідається про створення та встановлення celestia-node. Цей підручник передбачає, що ви виконали кроки з налаштування середовища розробки [тут](./environment.md).

## Установка ноди Celestia

### Встановлення Arabica

Встановлення celestia-node для Arabica Devnet означає встановлення певної версії, сумісної з мережею.

Встановіть двійковий файл celestia-node, виконавши такі команди:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Переконайтеся, що двійковий файл працює, і перевірте версію за допомогою команди `celestia version`:

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Встановлення Mamaki

Встановлення celestia-node для Mamaki Testnet означає встановлення певної версії, сумісної з мережею.

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

## Вибір мережі

Ви можете виконати вибір мережі у celestia-node між Arabica та Mamaki. Однак слід зауважити, що мережі найкраще працюють на версіях celestia-node, згаданих вище.

```sh
celestia light init
celestia light start --node.network arabica
```
