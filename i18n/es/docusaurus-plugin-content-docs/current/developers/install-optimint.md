---
sidebar_label: Instalando Optimint
---

# Configurando Optimint

Antes de continuar con la construcción de nuestra aplicación Wordle, tenemos que configurar Optimint en nuestro código.

## Instalando Optimint

Ejecuta los siguientes comandos en el directorio `js`.

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy
go mod download
```

Con esto, hemos añadido los cambios de Optimint al directorio del proyecto. ¡Ahora, vamos a construir la aplicación Wordle!
