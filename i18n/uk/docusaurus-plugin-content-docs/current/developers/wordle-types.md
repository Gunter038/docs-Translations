---
sidebar_label: Типи
---

# Типи Wordle

Для наступних кроків ми створимо типи, які використовуватимуться у створених нами повідомленнях.

## Різноманітні Типи Wordle

```sh
ignite scaffold map wordle word submitter --no-message
```

Цей тип є картою `Wordle` з двома значеннями `word` і `submitter`. `submitter` — це адреса особи, яка надіслала Wordle.

Другий тип - це тип `Guess`. Він дозволяє нам зберігати останні припущення для кожної адреси, яка надіслала рішення.

```sh
ignite scaffold map guess word submitter count --no-message
```

Тут ми також зберігаємо `count`, щоб підрахувати, скільки здогадів надіслала ця адреса.
