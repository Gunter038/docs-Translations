---
sidebar_label: Keeper
---

# Функції Keeper
<!-- markdownlint-disable MD013 -->

Тепер настав час реалізувати функції Keeper для кожного повідомлення. У документах Cosmos-SDK [Keeper](https://docs.cosmos.network/master/building-modules/keeper.html) визначається так:

> Основним ядром модуля Космос SDK є інструмент, що називається "keeper"(хранитель). Keeper обробляє взаємодію зі сховищем, має посилання на інших хранителів для взаємодії між модулями та містить більшість основних функцій модуля.

Keeper — це абстракція в Cosmos, яка дозволяє нам взаємодіяти зі сховищем ключів і значень і змінювати стан блокчейну.

Тут він допоможе нам окреслити логіку для кожного повідомлення, яке ми створили.

## Функція SubmitWordle

Спочатку ми починаємо з функції `SubmitWordle`.

Відкрийте наступний файл: `x/wordle/keeper/msg_server_submit_wordle.go`

Всередині наступного коду додайте наступний код, який ми розглянемо детально:

```go
package keeper

import (
  "context"
  "crypto/sha256"
  "encoding/hex"
  "github.com/YazzyYaz/wordle/x/wordle/types"
  sdk "github.com/cosmos/cosmos-sdk/types"
  sdkerrors "github.com/cosmos/cosmos-sdk/types/errors"
  "time"
  "unicode"
)

func (k msgServer) SubmitWordle(goCtx context.Context, msg *types.MsgSubmitWordle) (*types.MsgSubmitWordleResponse, error) {
  ctx := sdk.UnwrapSDKContext(goCtx)
  // Check to See the Wordle is 5 letters
  if len(msg.Word) != 5 {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Wordle Must Be A 5 Letter Word")
  }
  // Check to See Only Alphabets Are Passed for the Wordle
  if !(IsLetter(msg.Word)) {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Wordle Must Only Consist Of Letters In The Alphabet")
  }

  // Use Current Day to Create The Index of the Newly-Submitted Wordle of the Day
  currentTime := time.Now().Local()
  var currentTimeBytes = []byte(currentTime.Format("2006-01-02"))
  var currentTimeHash = sha256.Sum256(currentTimeBytes)
  var currentTimeHashString = hex.EncodeToString(currentTimeHash[:])
  // Hash The Newly-Submitted Wordle of the Day
  var submittedSolutionHash = sha256.Sum256([]byte(msg.Word))
  var submittedSolutionHashString = hex.EncodeToString(submittedSolutionHash[:])

  var wordle = types.Wordle{
    Index:     currentTimeHashString,
    Word:      submittedSolutionHashString,
    Submitter: msg.Creator,
  }

  // Try to Get Wordle From KV Store Using Current Day as Key
  // This Helps ensure only one Wordle is submitted per day
  _, isFound := k.GetWordle(ctx, currentTimeHashString)
  if isFound {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Wordle of the Day is Already Submitted")
  }
  // Write Wordle to KV Store
  k.SetWordle(ctx, wordle)
  return &types.MsgSubmitWordleResponse{}, nil
}

func IsLetter(s string) bool {
  for _, r := range s {
    if !unicode.IsLetter(r) {
      return false
    }
  }
  return true
}
```

Тут, у функції Keeper `SubmitWordle`, ми робимо кілька речей:

* Спочатку ми переконаємося, що слово, подане на Wordle of the Day, містить 5 літер і використовує лише алфавіти. Це означає, що в рядку не можна подавати цілі числа.
* Потім ми створюємо хеш із поточного дня, коли було надіслано Wordle. Ми встановили цей хеш в індексі типу Wordle. Це дозволяє нам шукати будь-які припущення для цього Wordle для наступних припущень, які ми розглянемо далі.
* Потім ми перевіряємо, чи порожній індекс для сьогоднішньої дати. Якщо він не порожній, це означає, що Wordle уже надіслано. Пам’ятайте, що на день можна надсилати лише одне слово. Інші повинні вгадувати надіслане слово.
* У нас також є допоміжна функція, яка перевіряє, чи містить рядок лише символи алфавіту.

## Функція SubmitWordle

Наступна функція Keeper, яку ми додамо - ця: `x/wordle/keeper/msg_server_submit_guess.go`

Всередині наступного коду додайте наступний код, який ми розглянемо детально:

```go
package keeper

import (
  "context"
  "crypto/sha256"
  "encoding/hex"
  "github.com/YazzyYaz/wordle/x/wordle/types"
  sdk "github.com/cosmos/cosmos-sdk/types"
  sdkerrors "github.com/cosmos/cosmos-sdk/types/errors"
  "strconv"
  "time"
  "github.com/tendermint/tendermint/crypto"
)

func (k msgServer) SubmitGuess(goCtx context.Context, msg *types.MsgSubmitGuess) (*types.MsgSubmitGuessResponse, error) {
  ctx := sdk.UnwrapSDKContext(goCtx)
  // Check Word is 5 Characters Long
  if len(msg.Word) != 5 {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Guess Must Be A 5 Letter Word!")
  }

  // Check String Contains Alphabet Letters Only
  if !(IsLetter(msg.Word)) {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Guess Must Only Consist of Alphabet Letters!")
  }

  // Get Current Day to Pull Up Wordle of That Day As A Hash
  currentTime := time.Now().Local()
  var currentTimeBytes = []byte(currentTime.Format("2006-01-02"))
  var currentTimeHash = sha256.Sum256(currentTimeBytes)
  var currentTimeHashString = hex.EncodeToString(currentTimeHash[:])
  wordle, isFound := k.GetWordle(ctx, currentTimeHashString)
  if !isFound {
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Wordle of The Day Hasn't Been Submitted Yet. Feel Free to Submit One!")
  }

  // We Convert Current Day and Guesser to A Hash To Use As An Index For Today's Guesses For That Guesser
  // That Way, A Person Can Guess 6 Times A Day For Each New Wordle Created
  var currentTimeGuesserBytes = []byte(currentTime.Format("2006-01-02") + msg.Creator)
  var currentTimeGuesserHash = sha256.Sum256(currentTimeGuesserBytes)
  var currentTimeGuesserHashString = hex.EncodeToString(currentTimeGuesserHash[:])
  // Hash The Guess To The Wordle
  var submittedSolutionHash = sha256.Sum256([]byte(msg.Word))
  var submittedSolutionHashString = hex.EncodeToString(submittedSolutionHash[:])

  // Get the Latest Guess entry for this Submitter for the current Wordle of the Day
  var count int
  guess, isFound := k.GetGuess(ctx, currentTimeGuesserHashString)
  if isFound {
    // Check if Submitter Reached 6 Tries
    if guess.Count == strconv.Itoa(6) {
      return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "You Have Guessed The Maximum Amount of Times for The Day! Try Again Tomorrow With A New Wordle.")
    }
    currentCount, err := strconv.Atoi(guess.Count)
    if err != nil {
      panic(err)
    }
    count = currentCount
  } else {
    // Initialize Count Value If No Entry Exists for this Submitter for Today's Wordle
    count = 0
  }
  // Increment Guess Count
  count += 1
  var newGuess = types.Guess{
    Index:     currentTimeGuesserHashString,
    Submitter: msg.Creator,
    Word:      submittedSolutionHashString,
    Count:     strconv.Itoa(count),
  }
  // Remove Current Guess Entry to be Updated With New Entry
  k.RemoveGuess(ctx, currentTimeGuesserHashString)
  // Add New Guess Entry
  k.SetGuess(ctx, newGuess)
  // Setup Reward 
  reward := sdk.Coins{sdk.NewInt64Coin("WORDLE", 100)}
  if !(wordle.Word == submittedSolutionHashString) {
    return &types.MsgSubmitGuessResponse{Title: "Wrong Answer", Body: "Your Guess Was Wrong. Try Again"}, nil
  } else {
    // If Submitter Guesses Correctly
    guesserAddress, _ := sdk.AccAddressFromBech32(msg.Creator)
    moduleAcct := sdk.AccAddress(crypto.AddressHash([]byte(types.ModuleName)))
    // Send Reward
    k.bankKeeper.SendCoins(ctx, guesserAddress, moduleAcct, reward) 
    return &types.MsgSubmitGuessResponse{Title: "Correct", Body: "You Guessed The Wordle Correctly!"}, nil
  }
}
```

У наведеному вище коді ми робимо такі речі:

* Тут ми знову виконуємо початкові перевірки слова, щоб переконатися, що воно містить 5 символів і в ньому використовуються лише символи алфавіту, які можна буде оновити в майбутньому або перевірити в командах CLI.
* Потім ми отримуємо слово дня, отримуючи хеш-рядок поточного дня.
* Далі ми створимо хеш-рядок поточного дня і Submitter. Це дозволяє нам створити тип Guess з індексом, який використовує поточний день і адресу відправника. Це допомагає нам, коли ми стикаємося з новим днем і адреса хоче вгадати нове слово дня. Налаштування індексу гарантує, що вони можуть продовжувати вгадувати нове слово щодня максимум до 6 спроб на день.
* Потім ми перевіряємо, чи цей тип припущення для Submitter для сьогоднішнього Wordle досяг 6 одиниць. Якщо ні, ми збільшимо рахунок. Потім ми перевіряємо, чи правильне число. Ми зберігаємо тип Guess з оновленим підрахунком стану.

## Protobuf File

  Декілька файлів для цієї роботи потрібно буде змінити.

Перший із них - `proto/wordle/tx.proto`.

Всередині цього файлу заповніть пустий `MsgSubmitGuessResponse` наступним кодом:

```go
message MsgSubmitGuessResponse {
  string title = 1;
  string body = 2;
}
```

Наступний файл - `x/wordle/types/expected_keepers.go`

Тут нам потрібно додати метод SendCoins до інтерфейсу BankKeeper, щоб дозволити надсилати винагороду правильному вгадувачу.

```go
type BankKeeper interface {
  SendCoins(ctx sdk.Context, fromAddr sdk.AccAddress, toAddr sdk.AccAddress, amt sdk.Coins) error
}
```

За допомогою цього ми реалізували всі наші функції Keeper! Час компілювати блокчейн і забрати його на тест-драйв.
