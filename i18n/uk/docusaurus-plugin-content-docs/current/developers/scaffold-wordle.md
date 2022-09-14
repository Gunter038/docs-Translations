---
sidebar_label: Риштування Ланцюга
---

# Ignite та Scaffolding Wordle Chain
<!-- markdownlint-disable MD013 -->

## Ignite

Ignite — чудовий інструмент CLI, який допоможе нам розпочати створення власних блокчейнів для додатків cosmos-sdk. Він надає безліч потужних інструментів і каркасів для додавання повідомлень, типів і модулів із набором бібліотек cosmos-sdk.

Ви можете прочитати більше про Ignite [тут](https://docs.ignite.com/).

Щоб встановити Ignite, ви можете виконати цю команду у вашому терміналі:

```sh
curl https://get.ignite.com/cli | bash
sudo mv ignite /usr/local/bin/
```

Це встановить Ignite CLI на вашій локальній машині. Цей підручник використовує MacOS, але він повинен працювати для Windows. Для користувачів Windows ознайомтеся з документацією Ignite щодо встановлення для машин Windows.

Тепер оновіть свій термінал за допомогою `source` або відкрийте новий термінальний сеанс, щоб зміни відбулися.

If you run the following:

```sh
ignite --help
```

You should see an output of help commands meaning Ignite was installed successfully!

## Scaffolding the Wordle Chain

Now, comes the fun part, creating a new blockchain! With Ignite, the process is pretty easy and straightforward.

Ignite CLI comes with several scaffolding commands that are designed to make development more straightforward by creating everything you need to build your blockchain.

First, we will use Ignite CLI to build the foundation of a fresh Cosmos SDK blockchain. Ignite minimizes how much blockchain code you must write yourself. If you are coming from the EVM-world, think of Ignite as a Cosmos-SDK version of Foundry or Hardhat but specifically designed to build blockchains.

We first run the following command to setup our project for our new blockchain, Wordle.

```sh
ignite scaffold chain github.com/YazzyYaz/wordle --no-module
```

This command scaffolds a new chain directory called `wordle` in your local directory from which you ran the command. Notice that we passed the `--no-module` flag, this is because we will be creating the module after.

## Wordle Directory

Now, it’s time to enter the directory:

```sh
cd wordle
```

Inside you will see several directories and architecture for your cosmos-sdk blockchain.

| File/directory | Purpose                                                                                                                                                                 |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| app/           | Files that wire together the blockchain. The most important file is `app.go` that contains type definition of the blockchain and functions to create and initialize it. |
| cmd/           | The main package responsible for the CLI of compiled binary.                                                                                                            |
| docs/          | Directory for project documentation. By default, an OpenAPI spec is generated.                                                                                          |
| proto/         | Protocol buffer files describing the data structure.                                                                                                                    |
| testutil/      | Helper functions for testing.                                                                                                                                           |
| vue/           | A Vue 3 web app template.                                                                                                                                               |
| x/             | Cosmos SDK modules and custom modules.                                                                                                                                  |
| config.yml     | A configuration file for customizing a chain in development.                                                                                                            |
| readme.md      | A readme file for your sovereign application-specific blockchain project.                                                                                               |

Going over each one is outside the scope of this guide, but we encourage you to read about it [here](https://docs.ignite.com/kb).

Most of the tutorial work will happen inside the `x` directory.
