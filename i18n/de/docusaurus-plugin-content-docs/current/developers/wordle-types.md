---
sidebar_label: Typen
---

# Wordle-Typen

Für die nächsten Schritte werden wir Typen erstellen, die von den von uns erstellten Nachrichten verwendet werden.

## Scaffolding Wordle-Typen

```sh
ignite scaffold map wordle word submitter --no-message
```

Dieser Typ ist eine Map namens `Word` mit zwei Werten von `word` und `submitter`. `submitter` ist die Adresse der Person, die das Wordle eingereicht hat.

Der zweite Typ ist der Typ `Guess`. Es ermöglicht uns, die neueste Vermutung für jede Adresse zu speichern, die eine Lösung eingereicht hat.

```sh
ignite scaffold map guess word submitter count --no-message
```

Hier speichern wir auch `count`, um zu zählen, wie viele Vermutungen diese Adresse abgegeben hat.
