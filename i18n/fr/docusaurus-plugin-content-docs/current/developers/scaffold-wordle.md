---
sidebar_label: Scaffolder la chaîne
---

# Ignite et Scaffolder la chaîne Wordle
<!-- markdownlint-disable MD013 -->

## Ignite

Ignite est un outil CLI (Interface de Ligne de Commande) incroyable pour nous aider à construire nos propres blockchains et applications cosmos-sdk. Il fournit de nombreux outils et scaffoldages pour ajouter des messages, types et modules avec bon nombre de bibliothèques cosmos-sdk.

Vous pouvez en apprendre davantage sur Ignite [ici](https://docs.ignite.com/).

Pour installer Ignite, vous pouvez exécuter cette commande sur votre terminal :

```sh
curl https://get.ignite.com/cli | bash
sudo mv ignite /usr/local/bin/
```

Cela installe Ignite CLI sur votre machine. Ce tutoriel utilise le système d'exploitation de Mac mais cela devrait fonctionner sur Windows. Pour les utilisateurs de Windows, consultez les documents d'Ignite relatifs à l'installation sur les machines Windows.

Maintenant, actualisez votre terminal en utilisant `source` ou ouvrez un nouveau terminal pour que le changement ait lieu.

Si vous exécutez la commande suivante :

```sh
ignite --help
```

Vous devriez voir une sortie de commandes d'aide affichant qu'Ignite a été installé avec succès !

## Scaffolder la chaîne Wordle

Maintenant vient la partie amusante : créer une nouvelle blockchain ! Avec Ignite, le processus est assez facile et direct.

Ignite CLI vient avec plusieurs "scaffoldings" conçus pour rendre le développement plus direct, en mettant à disposition tout ce dont vous avez besoin pour construire votre blockchain.

Premièrement nous allons utiliser Ignite CLI pour construire les fondations d'une nouvelle blockchain Cosmos SDK. Ignite réduit la quantité de code que vous devez écrire vous-même. Si vous venez du monde d'Ethereum et de sa machine virtuelle, vous pouvez voir Ingite comme la version Cosmos-SDK de la Foundry ou de la Hardhat mais spécifiquement conçue pour créer des blockchains.

Nous lançons d'abord la commande ci-dessous pour configurer notre projet de nouvelle blockchain : Wordle.

```sh
ignite scaffold chain github.com/YazzyYaz/wordle --no-module
```

Cette commande échafaude un nouveau répertoire appelé `wordle` dans votre répertoire local duquel vous avez exécuté la commande. Notez que nous avons entré le drapeau `--no-module`, parce que nous allons créer le module par la suite.

## Répertoire Wordle

Maintenant, il est temps d'entrer dans le répertoire :

```sh
cd wordle
```

A l'intérieur, vous verrez plusieurs répertoires et architectures pour votre blockchain cosmos-sdk compatible.

| Fichier/répertoire | Définition                                                                                                                                                                                  |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| app/               | Ce sont les fichiers qui relient la blockchain. Le fichier le plus important est `app.go` qui contient la définition du type de blockchain et les fonctions pour la créer et l'initialiser. |
| cmd/               | Package principal responsable du CLI du binaire compilé.                                                                                                                                    |
| docs/              | Répertoire pour toute la documentation relative au projet. Par défaut, une spécification OpenAPI est générée.                                                                               |
| proto/             | Fichiers tampon du protocole décrivant la structure des données.                                                                                                                            |
| testutil/          | Fonctions d'aide pour les tests.                                                                                                                                                            |
| vue/               | Un modèle d'application web Vue 3.                                                                                                                                                          |
| x/                 | Modules Cosmos SDK et modules personnalisés.                                                                                                                                                |
| config.yml         | Un fichier de configuration pour personnaliser une chaine en développement.                                                                                                                 |
| readme.md          | Un fichier lisez-moi pour votre projet de blockchain souveraine et spécifique à une application.                                                                                            |

Parcourir chacun de ces fichiers ne sera pas fait dans ce guide, mais nous vous encourageons à en lire davantage à ce sujet [ici](https://docs.ignite.com/kb).

La plupart de ce tutoriel se déroulera dans le répertoire `x`.
