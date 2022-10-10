# Rollmint

![rollmint](/img/rollmint.png)

[Rollmint](https://github.com/celestiaorg/rollmint) est une implémentation ABCI (Interface d'Application Blockchain) pour rollups souverains à déployer sur Celestia.

Il est construit en remplacement de Tendermint, la couche de consensus du Cosmos-SDK, avec une solution prête à l'usage qui communique directement avec la couche d'Accessibilité des Données de Celestia.

Il fait tourner un rollup souverain, qui collecte les transactions dans les blocs et les publie sur Celestia pour le consensus et l'accessibilité des données.

L'objectif de Rollmint est de permettre à n'importe qui de concevoir et déployer un rollup souverain sur Celestia en quelques minutes.

En outre, alors que Rollmint vous permet de construire des rollups souverains sur Celestia, il ne supporte actuellement pas les preuves de fraude et fonctionne donc en mode "pessimiste", dans lequel les noeuds doivent réexécuter les transactions pour vérifier la validité de la chaine (c.à.d un full node). Enfin, Rollmint ne supporte actuellement qu'un seul séquenceur.

## Tutoriels

Les tutoriels suivants vont vous aider à débuter la construction d'applications Cosmos-SDK qui connectent la couche d'accessibilité des données de Celestia via Rollmint. Nous appelons ces chaines les Rollups Souverains.

Vous pouvez commencer avec les tutoriels suivants :

- [Jeu du Wordle](./wordle.md)
- [Tutoriel CosmWasm](./cosmwasm.md)
