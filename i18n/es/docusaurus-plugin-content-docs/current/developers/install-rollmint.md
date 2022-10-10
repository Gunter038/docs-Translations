---
sidebar_label: Installing Rollmint
---

# Setting Up Rollmint

Before we continue with building our Wordle App, we need to set up Rollmint on our codebase.

## Installing Rollmint

Ejecuta los siguientes comandos en el directorio `js`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy
go mod download
```

With that, we have Rollmint changes added to the project directory. ¡Ahora, vamos a construir la aplicación Wordle!
