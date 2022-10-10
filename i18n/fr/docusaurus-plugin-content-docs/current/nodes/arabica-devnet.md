---
sidebar_label: Devnet Arabica
---

# Devnet Arabica
<!-- markdownlint-disable MD013 -->

![arabica-devnet](/img/arabica-devnet.png)

Le Devnet Arabica est un nouveau testnet de Celestia Labs qui se concentre exclusivement sur la fourniture aux développeurs de performances améliorées et les dernières mises à jour pour tester leurs rollups et applications.

Arabica ne se concentre pas sur les tests de validateurs ou de consensus, c'est plutôt le Testnet Mamaki qui est utilisé pour cela. Si vous êtes un validateur, nous vous recommandons de tester simplement vos opérations de validateur sur Mamaki [ici](./mamaki-testnet.md).

Avec Arabica qui dispose des dernières mises à jour de tous les produits Celestia déployés dessus, les changements peuvent être nombreux. Par conséquent, en guise d'avertissement transparent, Arabica peut se casser de manière inattendue mais étant donné qu'il sera continuellement mis à jour, c'est un moyen utile de continuer à tester les derniers changements sur le logiciel.

Les développeurs peuvent toujours déployer sur Mamaki leurs rollups souverains s'ils ont choisi de le faire, l'approche sera juste toujours à la traîne derrière Arabica Devnet, jusqu'à ce que Mamaki subisse des Hardforks en coordination avec les Validateurs.

## Intégrations

Ce guide contient les sections pertinentes pour savoir comment se connecter à Arabica, selon le type de node que vous exécutez.

Votre meilleure approche pour participer est de déterminer d'abord quel node vous aimeriez exécuter. Chaque guide de node sera lié au réseau concerné afin de vous montrer comment vous y connecter.

Vous avez une liste d'options sur le type de nodes que vous pouvez exécuter afin de participer à Arabica:

Disponibilité des données :

* [Bridge Node](./bridge-node.md)
* [Full Storage Node](./full-storage-node.md)
* [Light Node](./light-node.md)

Sélectionnez le type de node que vous souhaitez exécuter et suivez les instructions sur chaque page respective. À chaque fois que l'on vous demande de sélectionner le type de réseau auquel vous voulez vous connecter dans ces guides, sélectionnez `Arabica` afin de vous référer aux instructions correctes sur cette page sur la façon de vous connecter à Arabica.

## Points de terminaison RPC

Il y a une liste des terminaux RPC que vous pouvez utiliser pour vous connecter au Testnet Mamaki:

* [https://limani.celestia-devops.dev](https://limani.celestia-devops.dev)

## Faucet du Devnet Arabica

> L'UTILISATION DE CE FAUCET NE VOUS DONNERA PAS LE DROIT À UN AIRDROP OU UNE AUTRE DISTRIBUTION DE TOKENS DU MAINNET CELESTIA. LES JETONS DU MAINNET CELESTIA N'EXISTENT ACTUELLEMENT PAS ET IL N'Y A AUCUNE VENTE PUBLIQUE OU AUTRE DISTRIBUTION DE TOKENS DU MAINNET CELESTIA.

Vous pouvez demander au Faucet du Devnet Arabica sur le canal #faucet du serveur Discord de Celestia avec la commande suivante:

```text
$request <CELESTIA-ADDRESS>
```

Où `<CELESTIA-ADDRESS>` est une adresse générée par `celestia1******`.

> Remarque : Le Faucet a une limite de 10 jetons par semaine par adresse/ID Discord

## Explorateur

Il y a un explorateur que vous pouvez utiliser pour Arabica:

* [https://explorer.celestia.observer/arabica](https://explorer.celestia.observer/arabica)
