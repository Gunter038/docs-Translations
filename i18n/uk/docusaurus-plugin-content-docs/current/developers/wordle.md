---
sidebar_label: Огляд Wordle
---

# Wordle App on Rollmint

![mamaki-testnet](/img/wordle.jpg)

This tutorial guide will go over building a cosmos-sdk app for Rollmint, the Sovereign-Rollup implementation of Tendermint, for the popular game [Wordle](https://www.nytimes.com/games/wordle/index.html).

This tutorial will go over how to setup Rollmint in the Ignite CLI and use it to build the game. Підручник розгляне простий дизайн, а також завершить майбутні реалізації та ідеї щодо розширення цієї кодової бази.

> NOTE: This tutorial will explore developing with Rollmint, which is still in Alpha stage. Якщо ви виявите помилки, будь ласка, напишіть заявку на Github Issue або повідомте нам про це в нашому Discord. Furthermore, while Rollmint allows you to build sovereign rollups on Celestia, it currently does not support fraud proofs yet and is therefore running in "pessimistic" mode, where nodes would need to re-execute the transactions to check the validity of the chain (i.e. a full node). Furthermore, Rollmint currently only supports a single sequencer.

## Передумови

Оскільки цей підручник призначений для розробників, які мають досвід роботи з Cosmos-SDK, ми рекомендуємо вам переглянути наступні підручники в Ignite, щоб зрозуміти всі різні компоненти Cosmos-SDK, перш ніж продовжити цей підручник.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Основи блогу та модуля](https://docs.ignite.com/guide/blog)
* [Навчальний посібник зі служби імен](https://docs.ignite.com/guide/nameservice)
* [Полювання на сміття](https://docs.ignite.com/guide/scavenge)

Вам не обов’язково виконувати ці посібники, щоб слідувати цьому посібнику Wordle, але це допоможе вам краще зрозуміти архітектуру Cosmos-SDK.

## Імплементація розробки

Правила Wordle прості: Ви повинні вгадати слово дня.

Ключові точки для розгляду:

* У слові п'ять літер.
* У вас є 6 здогадок.
* Кожні 24 години - нове слово.

Графічний інтерфейс для Wordle показує кілька індикаторів: виділення зеленим кольором літери в певній позиції означає, що це правильна літера для Wordle у правильній позиції. Жовте виділення означає, що це правильна літера для Wordle, але в неправильній позиції. Сіра підсвічування означає, що буква не є частиною Wordle.

Для простоти дизайну ми уникатимемо цих підказок, хоча є способи розширити цю кодову базу для реалізації того, що ми покажемо наприкінці.

У поточній розробці ми реалізуємо такі правила:

* На день можна надсилати 1 Wordle;
* В кожній адресі буде 6 спроб вгадати слово;
* Це має бути п'ятибуквене слово;
* Той, хто правильно вгадає слово до закінчення 6 спроб, отримує нагороду в 100 токенів WORDLE.

Щоб досягти цього, ми розглянемо архітектуру далі в посібнику. Але зараз ми розпочнемо налаштування нашого середовища розробника.

## Зміст для цього посібника

Цей підручник розбитий на такі розділи:

1. [Ignite та Scaffolding ланцюга](./scaffold-wordle.md)
2. [Installing Rollmint](./install-rollmint.md)
3. [Модулі](./wordle-module.md)
4. [Повідомлення](./wordle-messages.md)
5. [Типи](./wordle-types.md)
6. [Хранителі](./wordle-keeper.md)
7. [Запуск Wordle](./run-wordle.md)
