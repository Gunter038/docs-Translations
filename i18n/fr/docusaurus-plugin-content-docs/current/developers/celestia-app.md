- - -
installer Celestia App
- - -

# Celestia App
<!-- markdownlint-disable MD013 -->

Ce didacticiel vous guidera dans l'installation de Celestia App. Ce tutoriel suppose que vous avez terminé les étapes de configuration de votre propre environnement [ici](./environment.md).

## Installer Celestia App

Les étapes ci-dessous créeront un fichier binaire nommé `celestia-appd` à l'intérieur du dossier `$HOME/go/bin` qui sera utilisé plus tard pour exécuter le node.

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

Pour vérifier si le binaire a été compilé avec succès, vous pouvez exécuter le binaire en utilisant l'option `--help`:

```sh
celestia-appd --help
```

Vous devriez voir une sortie similaire (avec des exemples de commandes utiles):

```text
Démarrer l'application Celestia 

Utilisation:
  celestia-appd [command]

Commandes disponibles :
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

Utilisez "celestia-appd [command] --help" pour plus d'informations à propos d'une commande.
```
