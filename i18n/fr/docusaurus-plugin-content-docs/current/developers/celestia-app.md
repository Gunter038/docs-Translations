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
  add-genesis-account   Ajoute un compte genesis à genesis.json
  collect-gentxs      Collecte les txs genesis et renvoie un fichier genesis.json 
  config              Créer ou interroger un fichier de configuration CLI d'application
  debug              Outil pour vous aider à déboguer votre application
  export              Exporter l'état vers JSON
  gentx               Génère une tx genesis portant une auto-délégation
  help                Aide sur n'importe quelle commande
  init                nitialiser les fichiers de configuration du validateur privé, p2p, genesis et de l'application
  keys                Gérez les clés de votre application
  migrate             Migrer Genesis vers une version cible spécifiée
  query               Interrogation des sous-commandes
  rollback            rollback tendermint d'une hauteur
  rollback            rollback l'état du cosmos-sdk et de tendermint d'une hauteur
  start               Execute le full node
  status              Query remote node for status
  tendermint          Sous-commandes de Tendermint 
  tx                  Sous-commandes de transactions
  validate-genesis    valide le fichier genesis à l'emplacement par défaut ou à l'emplacement passé en argument
  version             Afficher les informations sur la version binaire de l'application

Flags:
  -h, --help                Aide pour celestia-appd
      --home string         directoire pour configuration et donnéesa (default "/root/.celestia-app")
      --log_format string   Le format de journalisation  (json|plain) (par défaut "plain")
      --log_level string    Le niveau de journalisation (trace|debug|info|warn|error|fatal|panic) (par défaut "info")
      --trace               affiche la trace complète de la pile en cas d'erreur

Utilisez "celestia-appd [command] --help" pour plus d'informations à propos d'une commande.
```
