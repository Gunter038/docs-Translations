- - -
sidebar_label : Установка ноды Celestia
- - -

# Нода Celestia

В этом руководстве рассматривается создание и установка ноды celestia. В этом руководстве предполагается, что вы выполнили шаги по настройке стартовой среды [ тут](./environment.md).

## Установка ноды Celestia

### Установка Arabica

Установка ноды Сelestia в Arabica Devnet означает установку определенной версии для совместимости с сетью.

Установить бинарный (исполняемый) файл для ноды Сelestia, можно с помощью следующих команд:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Убедитесь, что бинарный (исполняемый) файл работает, и проверьте версию с помощью команды `celestia version`:

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Установка Mamaki

Установка ноды Сelestia в Mamaki Devnet означает установку определенной версии для совместимости с сетью.

Установить бинарный (исполняемый) файл для ноды Сelestia, можно с помощью следующих команд:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Убедитесь, что бинарный (исполняемый) файл работает, и проверьте версию с помощью команды `celestia version`:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Network Selection

You can perform network selection in celestia-node between Arabica and Mamaki. However, you should note that networks work best on the celestia-node versions mentioned above.

```sh
celestia light init
celestia light start --node.network arabica
```
