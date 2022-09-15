---
sidebar_label: Keeper
---

# Keeper 函数
<!-- markdownlint-disable MD013 -->

现在是时候将在每条信息上执行 Keeper 函数了。 根据 Cosmos-SDK 文档, [Keeper](https://docs.cosmos.network/master/building-modules/keeper.html) 定义如下:

> Cosmos SDK 模块的主要核心被称为Keeper. Keeper 处理与 store的交互, 引导与其他Keeper进行跨模块交互，具备模块的大部分核心功能。

Keeper 是 Cosmos 的抽象化，使得我们可以与 Key-Value store 进行交互并改变区块链的状态。

在这方面，它将帮助我们勾勒出创建的每个信息的逻辑。

## SubmitWordle 函数

我们从 `SubmitWordle` 函数开始。

打开以下文件： `x/wordle/keeper/msg_server_submit_wordle.go`

在下面的内容中，添加以下代码，我们将在稍后进行讨论：

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

关于`SubmitWordle` Keeper 函数，我们需要确定几项内容：

* 我们首先要确保生成的"Wordle of the Day "的单词是 5个字母长，并且只使用字母。 也就是说不能在字符串中提交整数。
* 然后我们根据当天Wordle生成的时刻创建一个哈希值。 我们将这个哈希值设置为Wordle类型的索引。 这便于我们查询有关这个Wordle的任何猜测，而后续进行猜测。
* 然后我们检查今天日期的索引是否为空值。 如果它不是空值，就意味着已经有了一个Wordle 生成了。 请注意，一天只能生成一个Wordle。 每个人只能去猜测当天已生成的Wordle。
* 我们还在其中设置了一个辅助函数，用来检查一个字符串是否只包含字母字符。

## SubmitGuess 函数

我们会添加的下一个 Keeper 函数如下： `x/wordle/keeper/msg_server_submit_guess.go`

打开该文件，并添加以下代码，我们将在稍后解释：

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

我们在上述代码中，做了以下事情：

* 首先，我们再次对这个词进行初步检查，以确保它是5个字符，并且只使用字母，这可以在将来重构或在CLI命令中检查。
* 然后，我们通过获取当天日期的哈希字符串来获得当天的Wordle。
* 接下来我们创建了当天的哈希字符串以及Submitter（猜测人）。 这允许我们创建一个使用当前日期和Submitter（猜测人）地址的且可索引的猜测类型。 当新的一天和某个地址想要猜测当天的Wordle时，这可以帮助我们。 索引的设置确保他们可以每天猜测新的Wordle 但每天最多只能猜6次。
* 然后我们检查今天Wordle的猜测者的猜测类型是否达到了6次。 如果没有，我们就继续计数。 我们随后会检查猜测是否正确。 此后，我们会存储猜测类型与更新的计数。

## Protobuf File

  我们需要对一些文件进行调整以实现以上功能。

首先需要调整的是`proto/wordle/tx.proto`。

在这个文件中，用以下代码填写空白的 `MsgSubmitGuessResponse`：

```go
message MsgSubmitGuessResponse {
  string title = 1;
  string body = 2;
}
```

下一个文件是 `x/wordle/types/expected_keepers.go`

现在，我们需要将 SendCoins 方法添加到 BankKeeper 的 接口，以便将奖励发送给正确的猜测者。

```go
type BankKeeper interface {
  SendCoins(ctx sdk.Context, fromAddr sdk.AccAddress, toAddr sdk.AccAddress, amt sdk.Coins) error
}
```

通过以上步骤，我们执行了 Keeper 函数! 是时候编写区块链，并进行测试了。
