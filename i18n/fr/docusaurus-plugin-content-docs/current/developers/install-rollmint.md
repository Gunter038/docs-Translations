---
sidebar_label: Installer Rollmint
---

# Configurer Rollmint

Avant que nous continuions à construire notre application Wordle, nous avons besoin de configurer Rollmint dans notre codebase.

## Installer Rollmint

Exécuter la commande suivante dans le répertoire `wordle`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy
go mod download
```

Grâce à cela, nous avons les changements relatifs à Rollmint intégrés au répertoire du projet. Maintenant, construisons la Wordle App !
