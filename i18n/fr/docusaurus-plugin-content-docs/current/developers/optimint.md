# Optimint

![optimint](/img/optimint.png)

[Optimint](https://github.com/celestiaorg/optimint) est une implémentation d'IABC (Interface d'Application BlockChain) pour rollups souverains à déployer sur Celestia.

Il est construit en remplacement de Tendermint, la couche de consensus du Cosmos-SDK, avec une solution prête à l'usage qui communique directement avec la couche d'Accessibilité des Données de Celestia.

Il fait tourner un rollup souverain, qui collecte les transactions dans les blocs et les publie sur Celestia pour le consensus et l'accessibilité des données.

L'objectif d'Optimint est de permettre à n'importe qui de concevoir et déployer un rollup souverain sur Celestia en quelques minutes.

En outre, alors qu'Optimint permet de construire des rollups souverains sur Celestia, il ne supporte pour l'instant pas les preuves de fraude et s'exécute donc en mode "pessimiste", dans lequel les noeuds auraient besoin d'exécuter à nouveau les transactions pour vérifier la validité de la chaine (c-à-d un full node). En outre, Optimint ne supporte actuellement qu'un seul séquenceur.

## Tutoriels

Les tutoriels suivants vont vous aider à débuter la construction d'applications Cosmos-SDK capables de se connecter à la couche d'accessibilité des données de Celestia, via Optimint. Nous appelons ces chaines les Rollups Souverains.

Vous pouvez commencer avec les tutoriels suivants :

- [Jeu du Wordle](./wordle.md)
- [Tutoriel CosmWasm](./cosmwasm.md)
