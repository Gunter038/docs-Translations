- - -
sidebar_label : Instalar la aplicación Celestia
- - -

# Celestia App
<!-- markdownlint-disable MD013 -->

Este tutorial te guiará a través de la construcción de Celestia App. Este tutorial presupone que completaste los pasos para configurar tu propio entorno [aquí](./environment.md).

## Instalar celestia-app

Los siguientes pasos crearán un archivo binario llamado `celestia-appd` dentro de la carpeta `$HOME/go/bin` que se utilizará más tarde para ejecutar el nodo.

```sh
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app/
APP_VERSION=$(curl -s \
  https://api.github.com/repos/celestiaorg/celestia-app/releases/latest \
  | jq -r ".tag_name")
git checkout tags/$APP_VERSION -b $APP_VERSION
make install
```

Para comprobar si el binario fue compilado con éxito, puedes ejecutar el binario usando el parámetro `--help`:

```sh
celestia-appd --help
```

Deberías ver una salida similar (con comandos de ejemplo útiles):

```text
Start celestia app

Usage:
  celestia-appd [command]

Available Commands:
  add-genesis-account Add a genesis account to genesis.json
  collect-gentxs      Collect genesis txs and output a genesis.json file
  config              Create or query an application CLI configuration file
  debug               Tool for helping with debugging your application
  export              Export state to JSON
  gentx               Generate a genesis tx carrying a self delegation
  help                Help about any command
  init                Initialize private validator, p2p, genesis, and application configuration files
  keys                Manage your application's keys
  migrate             Migrate genesis to a specified target version
  query               Querying subcommands
  rollback            rollback tendermint state by one height
  rollback            rollback cosmos-sdk and tendermint state by one height
  start               Run the full node
  status              Query remote node for status
  tendermint          Tendermint subcommands
  tx                  Transactions subcommands
  validate-genesis    validates the genesis file at the default location or at the location passed as an arg
  version             Print the application binary version information

Flags:
  -h, --help                help for celestia-appd
      --home string         directory for config and data (default "/root/.celestia-app")
      --log_format string   The logging format (json|plain) (default "plain")
      --log_level string    The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
      --trace               print out full stack trace on errors

Use "celestia-appd [command] --help" for more information about a command.
```
