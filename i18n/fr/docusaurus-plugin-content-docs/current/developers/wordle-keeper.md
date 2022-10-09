---
sidebar_label: Gardien
---

# Les fonctions du Gardien
<!-- markdownlint-disable MD013 -->

Maintenant, il est temps d'ajouter les fonctions du Gardien pour chaque message. À partir de la documentation du Cosmos-SDK, un [Keeper](https://docs.cosmos.network/master/building-modules/keeper.html) est défini de la façon qui suit :

> Le noyau principal d'un module Cosmos-SDK est une pièce appelée le Gardien. Le gardien gère les intéractions avec le stockage, a des références à d'autres gardiens pour les intéractions entre modules et contient la plupart des fonctionnalités de base d'un module.

Un Gardien est une abstraction sur Cosmos qui nous permet d'interagir avec le stockage de Key-Value et de changer l'état de la blockchain.

Ici, il nous aidera à définir la logique de chaque message que nous créons.

## Fonction SubmitWordle

Nous allons commencer avec la fonction `SubmitWordle`.

Ouvrez le fichier suivant : `x/wordle/keeper/msg_server_submit_wordle.go`

A l'intérieur de ce qui suit, ajoutez le code suivant, que nous allons étudier plus loin :

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

Ici, dans la fonction du Gardien `SubmitWordle`, nous allons faire certaines choses :

* Nous nous assurons d'abord que le mot soumis pour le mot du jour est long de 5 caractères et qu'il n'utilise que des lettres. Cela signifie qu'aucun nombre entier ne peut être être soumis dans la chaine.
* Nous créons ensuite un hash à partir du jour en cours au moment où le mot a été soumis. Nous plaçons ce hash à l'index du type du Wordle. Cela nous permet de rechercher toutes les suppositions de ce mot pour les jeux suivants, que nous allons étudier ensuite.
* Nous vérifions ensuite si l'index de la date du jour est actuellement vide ou non. S'il n'est pas vide, cela signifie qu'un mot a déjà été soumis. Rappelez-vous qu'un seul mot seulement peut être soumis chaque jour. Tout le monde doit deviner le mot soumis.
* Nous avons également une fonction d'aide pour vérifier si une chaine de caractères contient seulement des caractères alphabétiques.

## La fonction SubmitGuess

La prochaine fonction de Gardien que nous allons ajouter est la suivante : `x/wordle/keeper/msg_server_submit_guess.go`

Ouvrez ce fichier et ajoutez le code suivant, que nous allons expliquer un peu plus bas :

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
    return nil, sdkerrors.Wrap(sdkerrors.ErrInvalidRequest, "Wordle of The Day Hasn't Been Submitted Yet.
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
  Feel Free to Submit One!")
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
    Try Again Tomorrow With A New Wordle.")
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

Dans le code ci-dessus, nous effectuons les choses suivantes :

* Ici, nous effectuons des vérifications initiales sur le mot pour nous assurer qu'il est fait de 5 caractères uniquement alphabétiques, ce qui peut être refactorisé dans le futur ou vérifié dans les commandes CLI.
* Nous obtenons ensuite le mot du jour en récupérant la chaine de hash du jour actuel.
* Ensuite, nous créons une chaîne de hash du jour courant et l'Emetteur. Cela nous permet de créer un type de devinette pour les mots proposés avec un index qui utilise le jour actuel et l'adresse de l'émetteur. Cela nous aide quand nous sommes confrontés à un nouveau jour et qu'une adresse veut deviner ce nouveau mot. La configuration de l'index permet de continuer à essayer de deviner un nouveau mot chaque jour jusqu'à un maximum de 6 essais par jour.
* Nous vérifions ensuite si le nombre d'essais de l'émetteur a bien atteint 6 pour aujourd'hui. Si ce n'est pas le cas, il incrémente l'essai. Nous vérifions ensuite si la tentative est correcte. Nous stockons la devinette avec le nombre d'essais mis à jour.

## Le fichier Protobuf

  Quelques fichiers doivent être modifiés pour que cela fonctionne.

Le premier est `proto/wordle/tx.proto`.

Dans ce fichier, remplissez le `MsgSubmitGuessResponse` vide avec le code suivant :

```go
message MsgSubmitGuessResponse {
  string title = 1;
  string body = 2;
}
```

Le fichier suivant est : `x/wordle/types/expected_keepers.go`

Ici, nous avons besoin d'ajouter la méthode SendCoins à l'interface BankKeeper afin de permettre l'envoi de récompense à la personne qui a deviné.

```go
type BankKeeper interface {
  SendCoins(ctx sdk.Context, fromAddr sdk.AccAddress, toAddr sdk.AccAddress, amt sdk.Coins) error
}
```

Avec cela, nous avons implémenté toutes nos fonctions de Gardien ! Il est temps de compiler la blockchain et de l'exposer pour un galop d'essai.
