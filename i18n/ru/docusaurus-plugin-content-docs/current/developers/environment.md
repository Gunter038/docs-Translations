- - -
sidebar_label : Настройка среды
- - -

# Среда разработки

В этом руководстве мы рассмотрим настройку рабочей среды для запуска ПО Celestia. Эта среда может использоваться для разработки, сборки исполняемых файлов и запуска ноды.

## Установка зависимостей

После того как вы настроили свой сервер, войдите в него по ssh ключу, чтобы начать установку необходимых файлов для работы ноды.

Во-первых, обязательно обновите и улучшите ОС:

```sh
# Если вы используете менеджер пакетов APT
sudo apt update && sudo apt upgrade -y

# Если вы используете менеджер пакетов YUM
sudo yum update
```

Это важные пакеты, которые необходимы для выполнения многих задач, таких как загрузка файлов, компиляция и мониторинг работы ноды:

<!-- markdownlint-disable MD013 -->
```sh
# Если вы используете менеджер пакетов APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y

# Если вы используете менеджер пакетов YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Установка Golang

Celestia-app и celestia-node написаны на [Golang](https://go.dev/), поэтому мы должны установить Golang, чтобы собрать и запустить их.

```sh
ver="1.19.1"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Теперь нам нужно добавить содержимое каталога `/usr/local/go/bin` в `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Чтобы убедиться, что Go был правильно установлен проверим версию:

```sh
go version
```

Вывод должен показать установленную версию:

```sh
go version go1.19.1 linux/amd64
```
