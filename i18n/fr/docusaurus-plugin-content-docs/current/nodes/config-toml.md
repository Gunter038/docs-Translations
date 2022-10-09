- - -
sidebar_label : Guide Config.toml
- - -

# Décomposition de Config.toml

- [Décomposition de Config.toml](#configtoml-breakdown)
  - [Pré-requis](#pre-requisites)
  - [Comprendre config.toml](#understanding-configtoml)
    - [[Core]](#core)
    - [[P2P]](#p2p)
      - [Bootstrap](#bootstrap)
      - [Peers mutuels](#mutual-peers)
    - [[Services]](#services)
      - [TrustedHash et TrustedPeer](#trustedhash-and-trustedpeer)

## Pré-requis

Veuillez vous assurer que vous avez installé et initialisé Celestia Node

## Comprendre config.toml

Après l'initialisation, pour n'importe quel type de node, vous trouverez un `config.toml` dans le chemin suivant (emplacement par défaut):

- `$HOME/.celestia-bridge/config.toml` pour un Bridge node
- `$HOME/.celestia-light/config.toml` pour un light node

Nous allons décomposer certaines des sections les plus utilisées.

### [Core]

Cette section est nécessaire pour le Bridge Node Celestia. Par défaut, `Remote = false`. Toujours pour un devnet, nous allons utiliser l'option remote core, cela peut également être défini avec la ligne de commande `--core.remote`.

### [P2P]

#### Bootstrap

Les bootstrappers aident les nouveaux nœuds à trouver des pairs plus rapidement dans le réseau. Par défaut, le `Bootstrapper = false` et `BootstrapPeers` est vide. Si vous voulez que votre node soit un "bootstrapper", activez `Bootstrapper = true`. Les `BootstrapPeers` sont déjà fournies par défaut lors de l'initialisation. Si vous souhaitez ajouter les vôtres manuellement, vous devez fournir le multiadresse des pairs.

#### Pairs mutuels

Le but de cette configuration est de mettre en place une communication bidirectionnelle. C'est généralement le cas pour les Bridge Node Celestia. De plus, vous aurez besoin de changer le champ `PeerExchange` de faux à vrai.

### [Services]

#### TrustedHash et TrustedPeer

`TrustedHash` est nécessaire pour initialiser correctement un Bridge node Celestia avec un Node Celestia Core `Remote` déjà en cours d'exécution. Le Light Node Celestia prendra un hash Genesis comme celui de confiance, s'il n'y a pas de hash fourni manuellement lors de la phase d'initialisation.

`Les TrustedPeers` sont la table des pairs de Bridge Nodes auxquels les lights nodes Celestia font confiance. Par défaut les pairs à bootstrap deviennent des pairs de confiance pour les Light Nodes Celestia si un utilisateur ne définit pas les paramètres des pairs de confiance dans le fichier de configuration.

Tout Bridge Node Celestia peut être un pair de confiance pour un Light. Toutefois, par son architecture, un Light Node ne peut pas être un trusted peer pour un autre Light Node.
