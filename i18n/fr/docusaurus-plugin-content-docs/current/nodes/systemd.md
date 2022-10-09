- - -
sidebar_label : SystemD
- - -

# Configuration de votre Node en tant que processus d'arrière-plan avec SystemD

SystemD est un service daemon utile pour exécuter des applications en tant que processus en arrière-plan.

## Consensus Nodes

Si vous exécutez un node validateur ou complet de consensus, voici les étapes pour configurer `celestia-appd` en tant que processus d'arrière-plan.

### Démarrer Celestia App avec SystemD

SystemD est un service daemon utile pour exécuter des applications en tant que processus en arrière-plan.

Créer un fichier systemd Celestia-App :

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-appd.service
[Unit]
Description=celestia-appd Cosmos daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia-appd start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
EOF
```

Si le fichier a été créé avec succès, vous pourrez voir son contenu :

```sh
cat /etc/systemd/system/celestia-appd.service
```

Activer et démarrer le daemon celestia-appd :

```sh
systemctl enable celestia-appd
systemctl start celestia-appd
```

Vérifier si le daemon a été démarré correctement :

```sh
systemctl status celestia-appd
```

Vérifier les logs du daemon en temps réel :

```sh
journalctl -u celestia-appd.service -f
```

Pour vérifier si votre node est synchronisé avant d'aller de l'avant :

```sh
curl -s localhost:26657/status | jq .result | jq .sync_info
```

Assurez-vous que vous avez `"catching_up": false`, sinon laissez-le tourner jusqu'à ce qu'il soit synchronisé.

## Nœuds de disponibilité des données

### Node de stockage complet Celestia

Créer un fichier systemd de Full Storage Node Celestia:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-full.service
[Unit]
Description=celestia-full Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia full start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Si le fichier a été créé avec succès, vous pourrez voir son contenu :

```sh
cat /etc/systemd/system/celestia-full.service
```

Activer et démarrer le daemon celestia-full:

```sh
systemctl enable celestia-full
systemctl start celestia-full && journalctl -u \
celestia-full.service -f
```

Vous devriez voir les logs à travers la synchronisation du full storage node.

### Bridge Node Celestia

Créer un fichier systemd Celestia Bridge :

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-bridge.service
[Unit]
Description=celestia-bridge Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia bridge start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Si le fichier a été créé avec succès, vous pourrez voir son contenu :

```sh
cat /etc/systemd/system/celestia-bridge.service
```

Activer et démarrer le daemon celestia-bridge:

```sh
systemctl enable celestia-bridge
systemctl start celestia-bridge && journalctl -u \
celestia-bridge.service -f
```

Maintenant, le bridge node Celestia va commencer à synchroniser les entêtes et stocker les blocs depuis l'application Celestia.

> Remarque : Au démarrage, nous pouvons voir le `multiadress` à partir du bridge node Celestia. Ceci est **nécessaire pour les futures connexions de Light Node** et la communication avec les bridge nodes Celestia

Exemple:

```sh
NODE_IP=<ip-address>
/ip4/$NODE_IP/tcp/2121/p2p/12D3KooWD5wCBJXKQuDjhXFjTFMrZoysGVLtVht5hMoVbSLCbV22
```

Vous devriez voir les logs à travers la synchronisation du bridge node.

### Light Node Celestia

Lancer le Light Node en tant que processus daemon en arrière-plan

<!-- markdownlint-disable MD013 -->
```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
[Unit]
Description=celestia-lightd Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia light start --core.ip <ip-address> --core.grpc.port <port>
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```
<!-- markdownlint-enable MD013 -->

Si un fichier a été créé avec succès, vous pourrez voir son contenu :

```sh
cat /etc/systemd/system/celestia-lightd.service
```

Activer et démarrer le daemon celestia-lightd :

```sh
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

Vérifier si le daemon a été démarré correctement :

```sh
systemctl status celestia-lightd
```

Vérifier les logs du daemon en temps réel :

```sh
journalctl -u celestia-lightd.service -f
```

Maintenant, le Light Node Celestia va commencer à synchroniser les en-têtes. Une fois la synchronisation terminée, le Light Node effectuera l'échantillonnage disponibilité des données (DAS) à partir du Bridge Node.
