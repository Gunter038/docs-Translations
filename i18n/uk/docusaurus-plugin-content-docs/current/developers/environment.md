- - -
sidebar_label : Налаштування середовища
- - -

# Середовище розробки

У цьому підручнику описано налаштування середовища розробки для запуску програмного забезпечення Celestia. Це середовище можна використовувати для розробки, створення двійкових файлів і запуску вузлів.

## Встановлення залежностей

Налаштувавши свій екземпляр, запустіть ssh в екземпляр, щоб почати встановлення залежностей, необхідних для запуску ноди.

Спочатку переконайтеся, що бажаєте оновити та покращити ОС:

```sh
# Якщо ви використовуєте менеджер пакетів APT
sudo apt update && sudo apt upgrade -y

# Якщо ви використовуєте менеджер пакетів YUM
sudo yum update
```

Це важливі пакети, які необхідні для виконання багатьох завдань, таких як завантаження файлів, компілювання і моніторинг ноди:

<!-- markdownlint-disable MD013 -->
```sh
# Якщо ви використовуєте менеджер пакетів APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Якщо ви використовуєте менеджер пакетів YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Інсталювати Golang

Celestia-app і celestia-node написані мовою [Golang](https://go.dev/), тому ми повинні встановити Golang, щоб створити та запустити їх.

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
go version go1.19.1 linux/amd64
```
