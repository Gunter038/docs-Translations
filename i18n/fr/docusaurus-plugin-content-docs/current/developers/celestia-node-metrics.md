# Métriques de Nœud Celestia

Ce tutoriel est pour exécuter des métriques pour votre instance de disponibilité des données de votre nœud Celestia.

Ce tutoriel se concentrera sur l'exécution de métriques pour un light node.

Ce tutoriel suppose que vous avez déjà configuré votre light node en suivant le [tutoriel de Node API](./node-tutorial.md).

## Drapeaux métriques en exécution

Vous pouvez activer les drapeaux métriques du nœud Celestia avec la commande suivante :

<!-- markdownlint-disable MD013 -->
```sh
celestia light start --core.ip <ip-address> --core.grpc.port <port> --metrics --metrics.endpoint <ip-address:port>
```
<!-- markdownlint-enable MD013 -->

Notez que les drapeaux `--metrics` activent les métriques et attendent une entrée au niveau de `--metrics.endpoint`.

Nous allons passer en revue ce que le terminal devra être dans la prochaine section.

## Réflexions de conception du terminal des métriques

Pour le moment, l'architecture des métriques au sein du noeud Celestia fonctionne comme spécifié dans l'[ADR](https://github.com/celestiaorg/celestia-node/blob/main/docs/adr/adr-010-incentivized-testnet-monitoring.md) suivant.

En substance, les réflexions sur la conception vont ici nécessiter d'exécuter un collecteur OpenTelemetry (OTEL) qui se connecte au light node Celestia.

Pour une présentation d'OTEL, consultez le guide [ici](https://opentelemetry.io/docs/collector/).

L'ADR et la document de l'OTEL vous aideront à exécuter votre collecteur sur le terminal des métriques. Cela vous permettra alors de traiter les données du collecteur sur un serveur Prometheus qui peut être consulté sur un tableau de bord Grafana.

Dans le futur, nous souhaitons des outils de développement en accès libre autour de cette infrastructure pour permettre aux opérateurs de nœud d'être en capacité de surveiller leurs nœuds de disponibilité des données.
