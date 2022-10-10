---
sidebar_label: Установка Optimint
---

# Настройка Optimint

Прежде чем мы продолжим создание нашего Wordle App, нам необходимо установить Optimint для нашей информационной базы.

## Установка Optimint

Выполните следующую команду в директории `wordle`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy
go mod download
```

Таким образом, мы добавили изменения Optimint в директорию проекта. Сейчас, давайте создадим Wordle app!
