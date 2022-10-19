---
sidebar_label: 種類
---

# 単語帳の種類

次のステップでは、作成したメッセージで使用される型を作成することになります。

## 足場となるワードルの種類

```sh
ignite scaffold map wordle word submitter --no-message
```

この型は `Wordle` と呼ばれるマップで、`word` と `submitter` の 2 つの値があります。 `submitter`は、Wordleを投稿した人のアドレスです。のアドレスです。

2つ目のタイプは`Guess`タイプです。 これにより、解答を提出した各住所の最新の推測を保存することができます。

```sh
ignite scaffold map guess word submitter count --no-message
```

ここでは、このアドレスが何回推測を提出したかをカウントするために、`count`も格納している。
