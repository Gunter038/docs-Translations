---
sidebar_label: Echafauder la chaine
---

# Ignite et échafauder la chaine Wordle
<!-- markdownlint-disable MD013 -->

## Ignite

Ignite is an amazing CLI tool to help us get started building our own blockchains for cosmos-sdk apps. It provides lots of power toolings and scaffoldings for adding messages, types, and modules with a host of cosmos-sdk libraries provided.

Vous pouvez en apprendre davantage sur Ingite [ici](https://docs.ignite.com/).

Pour installer Ignite, vous pouvez exécuter cette commande sur votre terminal :

```sh
curl https://get.ignite.com/cli | bash
sudo mv ignite /usr/local/bin/
```

This installs Ignite CLI in your local machine. Ce tutoriel utilise le système d'exploitation de Mac mais cela devrait fonctionner sur Windows. Pour les utilisateurs de Windows, consultez les documents d'Ignite relatifs à l'installation sur les machines Windows.

Maintenant, actualisez votre terminal en utilisant `source` ou ouvrez un nouveau terminal pour que le changement ait lieu.

Si vous exécutez la commande suivante :

```sh
ignite --help
```

Vous devriez alors voir une invite de commandes signifiant qu'Ignite a été installé avec succès !

## Scaffolding the Wordle Chain

Maintenant vient la partie amusante : créer une nouvelle blockchain ! Avec Ignite, le processus est assez facile et direct.

Ignite CLI comes with several scaffolding commands that are designed to make development more straightforward by creating everything you need to build your blockchain.

First, we will use Ignite CLI to build the foundation of a fresh Cosmos SDK blockchain. Ignite minimizes how much blockchain code you must write yourself. If you are coming from the EVM-world, think of Ignite as a Cosmos-SDK version of Foundry or Hardhat but specifically designed to build blockchains.

Nous lançons d'abord la commande ci-dessous pour configurer notre projet de nouvelle blockchain : Wordle.

```sh
ignite scaffold chain github.com/YazzyYaz/wordle --no-module
```

This command scaffolds a new chain directory called `wordle` in your local directory from which you ran the command. Notice that we passed the `--no-module` flag, this is because we will be creating the module after.

## Wordle Directory

Maintenant, il est temps d'entrer dans le répertoire :

```sh
cd wordle
```

A l'intérieur, vous verrez plusieurs répertoires et architectures pour votre blockchain cosmos-sdk compatible.

| Fichier/répertoire | Objectif                                                                                                                                                                |
| ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| app/               | Files that wire together the blockchain. The most important file is `app.go` that contains type definition of the blockchain and functions to create and initialize it. |
| cmd/               | The main package responsible for the CLI of compiled binary.                                                                                                            |
| docs/              | Directory for project documentation. By default, an OpenAPI spec is generated.                                                                                          |
| proto/             | Protocol buffer files describing the data structure.                                                                                                                    |
| testutil/          | Helper functions for testing.                                                                                                                                           |
| vue/               | A Vue 3 web app template.                                                                                                                                               |
| x/                 | Cosmos SDK modules and custom modules.                                                                                                                                  |
| config.yml         | Un fichier de configuration pour personnaliser une chaine en développement.                                                                                             |
| readme.md          | Un fichier lisez-moi pour votre projet de blockchain souveraine et spécifique à une application.                                                                        |

Parcourir chacun de ces fichiers ne sera pas fait dans ce guide, mais nous vous encourageons à en lire davantage à ce sujet [ici](https://docs.ignite.com/kb).

La plupart de ce tutoriel se déroulera dans le répertoire `x`.
