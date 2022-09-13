---
sidebar_label: Встановлення Optimint
---

# Налаштування Optimint

Перш ніж ми продовжимо створювати нашу програму Wordle, нам потрібно налаштувати Optimint на нашій кодовій базі.

## Встановлення Optimint

Запустити наступну команду всередині каталогу `wordle`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy
go mod download
```

Таким чином, у нас є зміни, додані до каталогу проєкту та оптимізації. Тепер збудуймо додаток Wordl!
