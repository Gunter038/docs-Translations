---
sidebar_label: Types
---

# Wordle Types

For the next steps, we will be creating types to be used by the messages we created.

## Scaffolding Wordle Types

```sh
ignite scaffold map wordle word submitter --no-message
```

This type is a map called `Wordle` with two values of `word` and `submitter`. `submitter` is the address of the person that submitted the Wordle.

The second type is the `Guess` type. It allows us to store the latest guess for each address that submitted a solution.

```sh
ignite scaffold map guess word submitter count --no-message
```

여기에 추측 횟수를 계산하기 위해 `count`를 저장했습니다. 이 주소가 제출되었습니다.
