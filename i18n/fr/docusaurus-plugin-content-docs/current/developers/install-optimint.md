---
sidebar_label: Installer Optimint
---

# Configurer Optimint

Avant que nous ne continuions de construire notre Wordle App, nous devons configurer Optimint dans notre codebase.

## Installer Optimint

Exécuter la commande suivante dans le répertoire `wordle`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy
go mod download
```

Grâce à cela, nous avons les changements d'Optimint ajoutés au répertoire du projet. Maintenant, construisons la Wordle App !
