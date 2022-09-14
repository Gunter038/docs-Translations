---
sidebar_label: Огляд Wordle
---

# Додаток Wordle на Optimint

![mamaki-testnet](/img/wordle.jpg)

У цьому навчальному посібнику описано створення програми cosmos-sdk для Optimint, реалізації Optimistic Rollup Tendermint для популярної гри [ Wordle](https://www.nytimes.com/games/wordle/index.html).

У цьому посібнику описано, як налаштувати Optimint в Ignite CLI та використовувати його для створення гри. Підручник розгляне простий дизайн, а також завершить майбутні реалізації та ідеї щодо розширення цієї кодової бази.

> ПРИМІТКА. У цьому підручнику розглядається розробка за допомогою Optimint, яка все ще перебуває на стадії альфа-версії. Якщо ви виявите помилки, будь ласка, напишіть заявку на Github Issue або повідомте нам про це в нашому Discord. Крім того, хоча Optimint дозволяє вам створювати суверенні зведені пакети на Celestia, він наразі ще не підтримує докази шахрайства, тому працює в «песимістичному» режимі, де ноди мають повторно виконати транзакції, щоб перевірити дійсність ланцюжка (тобто повну ноду). Крім того, наразі Optimint підтримує лише один секвенсер.

## Передумови

Оскільки цей підручник призначений для розробників, які мають досвід роботи з Cosmos-SDK, ми рекомендуємо вам переглянути наступні підручники в Ignite, щоб зрозуміти всі різні компоненти Cosmos-SDK, перш ніж продовжити цей підручник.

* [Hello, World](https://docs.ignite.com/guide/hello)
* [Основи блогу та модуля](https://docs.ignite.com/guide/blog)
* [Навчальний посібник зі служби імен](https://docs.ignite.com/guide/nameservice)
* [Полювання на сміття](https://docs.ignite.com/guide/scavenge)

Вам не обов’язково виконувати ці посібники, щоб слідувати цьому посібнику Wordle, але це допоможе вам краще зрозуміти архітектуру Cosmos-SDK.

## Імплементація розробки

Правила Wordle прості: Ви повинні вгадати слово дня.

Key Points to Consider:

* The word is a five-letter word.
* You have 6 guesses.
* Every 24 hours, there’s a new word.

The GUI for Wordle shows you a few indicators: a green highlight on a letter in a certain position means that’s the correct letter for the Wordle in the right position. A yellow highlight means it’s a correct letter for the Wordle included in the wrong position. A grey highlight means the letter isn’t part of the Wordle.

For simplicity of the design, we will avoid those hints, although there are ways to extend this codebase to implement that, which we will show at the end.

In this current design, we implement the following rules:

* 1 Wordle can be submitted per day
* Every address will have 6 tries to guess the word
* It must be a five-letter word.
* Whoever guesses the word correctly before their 6 tries are over gets an award of 100 WORDLE tokens.

We will go over the architecture to achieve this further in the guide. But for now, we will get started setting up our development environment.

## Table of Contents For This Tutorial

The following tutorial is broken down into the following sections:

1. [Ignite and Chain Scaffolding](./scaffold-wordle.md)
2. [Installing Optimint](./install-optimint.md)
3. [Modules](./wordle-module.md)
4. [Messages](./wordle-messages.md)
5. [Types](./wordle-types.md)
6. [Keepers](./wordle-keeper.md)
7. [Running Wordle](./run-wordle.md)
