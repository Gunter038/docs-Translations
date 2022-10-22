---
sidebar_label: Construyendo un Rollup Soberano
---

# 🗞Construyendo un Rollup Soberano

El CLI de Ignite viene con comandos de andamiaje para hacer que el desarrollo de blockchains sea más rápido creando todo lo necesario para iniciar una nueva blockchain de Cosmos SDK.

Abre una nueva pestaña o ventana en tu terminal y ejecuta este comando para andamiar el rollup:

```bash
ignite scaffold chain gm
```

La respuesta se verá similar a la siguiente:

```bash
jcs @ ~ % ignite scaffold chain gm

⭐️ Successfully created a new blockchain 'gm'.
👉 Get started with the following commands:

 % cd gm
 % ignite chain serve
```

Este comando ha creado una blockchain de Cosmos SDK en el directorio `gm`. El directorio `gm` contiene una blockchain completamente funcional. Los siguientes [módulos](https://docs.cosmos.network/master/modules/) de estándares Cosmos SDK han sido importados:

- `staking` - para mecanismo de consenso delegado de Proof-of-Stake (PoS)
- `bank` - para transferencias de tokens fungibles entre cuentas
- `gov` - para la gobernanza on-chain
- `mint` - para mintear nuevas unidades de token de staking
- `nft` - para crear, transferir y actualizar NFTs
- y [más](https://docs.cosmos.network/master/architecture/adr-043-nft-module.html)

Cambiar al directorio `gm`:

```bash
cd gm
```

Puedes aprender más sobre la estructura de archivos del directorio `gm` [aquí](https://docs.ignite.com/guide/hello#blockchain-directory-structure). La mayor parte del trabajo del tutorial se desarrollará dentro del directorio `x`.

## 💎Instalando Rollmint

Para cambiar Tendermint por Rollmint, ejecuta el siguiente comando:

```bash
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy
go mod download
```

## 🎬 Iniciando la blockchain

Ahora que tenemos nuestro andamio desplegablo y totalmente funcional, podemos iniciar nuestra cadena en nuestra máquina ejecutando este comando en el directorio `gm`:

```bash
ignite chain serve
```

La respuesta se verá similar a la siguiente:

```bash
La versión de Cosmos SDK es: stargate - v0.46.1

🛠️ Building proto...
📦 Installing dependencies...
🛠️  Building the blockchain...
💿 Initializing the app...
🙂 Created account "alice" with address
"cosmos1x68wdvng7w56h48t7tmnccfg84uxe76yppjc6j"
with mnemonic: "breezegarage boil under old useless vessel shoulder donkey
deputy ripple mention air remain find tent bright ill judge effort small lazy
salmon oppose"
🙂 Created account "bob" with address
"cosmos1uzwtd2lts0ak7dhha8d4gaqsd7ucph90gqdxrw"
with mnemonic: "excuse frozen level baby virus beauty pitch pill lobster argue
teach half loan argue wing daughter kit episode diary exhibit material fortune
learn wool"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

El comando `ignite chain serve` descarga las dependencias y compila el código fuente en un binario llamado `gmd` (repo + `d`). A partir de ahora, usarás `gmd` para ejecutar todos los comandos de cadena.

### 🛑 Deteniendo tu blockchain

Para detener tu blockchain, pulsa `Ctrl + C` en la ventana de terminal donde está ejecutándose. Estamos listos para preparar nuestra primera consulta de Rollup Soberano y conectar a la capa DA de Celestia.
