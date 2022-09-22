---
sidebar_label: Keeper
---

# Funciones de Keeper
<!-- markdownlint-disable MD013 -->

Ahora es el momento de implementar las funciones de Keeper para cada mensaje. De los documentos Cosmos-SDK, [Keeper](https://docs.cosmos.network/master/building-modules/keeper.html) se define como lo siguiente:

> El núcleo principal de un módulo Cosmos SDK es una pieza llamada keeper. Keeper maneja las interacciones con el almacenamiento, tiene referencias a otros mantenedores para interacciones entre módulos y contiene la mayoría de la funcionalidad principal de un módulo.

Keeper es una abstracción en Cosmos que nos permite interactuar con el almacén Key-Value y cambiar el estado de la blockchain.

Aquí, nos ayudará a esbozar la lógica de cada mensaje que creamos.

## Función SubmitWordle

Comenzamos primero con la función `SubmitWordle`.

Abra el siguiente archivo: `x/wordle/keeper/msg_server_submit_wordle.go`

Dentro del siguiente bloque, añade el siguiente código, el cual pasaremos en un rato:

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

Aquí en la función Keeper de `SubmitWordle`, estamos haciendo algunas cosas:

* Primero nos aseguramos de que una palabra enviada para la palabra del día tenga 5 letras de largo y solo use alfabetos. Eso significa que ningún entero puede ser enviado en la cadena.
* Entonces creamos un hash desde el día actual en el momento en que se envió la Wordle. Establecemos este hash al índice del tipo Wordle. Esta nos permite buscar cualquier suposición para esta Wordle, lo cual pasaremos a continuación.
* Entonces verificamos si el índice para la fecha de hoy está vacío o no. Si no está vacío, esto significa que un Wordle ya ha sido enviado. Recuerda, solo se puede enviar una wordle por día. Todos los demás tienen que adivinar la palabra presentada.
* También tenemos una función de ayuda ahí para comprobar si una cadena sólo contiene caracteres de alfabeto.

## Función SubmitGuess

La siguiente función de Keeper que añadiremos es la siguiente: `x/wordle/keeper/msg_server_submit_guess.go`

Dentro del siguiente bloque, añade el siguiente código, el cual pasaremos en un rato:

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
  Wrap(sdkerrors.ErrInvalidRequest, "Guess Must Be A 5 Letter Word!")
  }

  // Check String Contains Alphabet Letters Only
  if !(IsLetter(msg.Word)) {
    return nil, sdkerrors.
  Now().Local()
  var currentTimeBytes = []byte(currentTime.Format("2006-01-02"))
  var currentTimeHash = sha256.Sum256(currentTimeBytes)
  var currentTimeHashString = hex. EncodeToString(currentTimeHash[:])
  wordle, isFound := k. GetWordle(ctx, currentTimeHashString)
  if !isFound {
    return nil, sdkerrors. Wrap(sdkerrors.ErrInvalidRequest, "Wordle of The Day Hasn't Been Submitted Yet. Feel Free to Submit One!")
  Feel Free to Submit One!")
  }

  // We Convert Current Day and Guesser to A Hash To Use As An Index For Today's Guesses For That Guesser
  // That Way, A Person Can Guess 6 Times A Day For Each New Wordle Created
  var currentTimeGuesserBytes = []byte(currentTime.Format("2006-01-02") + msg. Creator)
  var currentTimeGuesserHash = sha256.Sum256(currentTimeGuesserBytes)
  var currentTimeGuesserHashString = hex. EncodeToString(currentTimeGuesserHash[:])
  // Hash The Guess To The Wordle
  var submittedSolutionHash = sha256.Sum256([]byte(msg.Word))
  var submittedSolutionHashString = hex. EncodeToString(submittedSolutionHash[:])

  // Get the Latest Guess entry for this Submitter for the current Wordle of the Day
  var count int
  guess, isFound := k. GetGuess(ctx, currentTimeGuesserHashString)
  if isFound {
    // Check if Submitter Reached 6 Tries
    if guess. Count == strconv. Itoa(6) {
      return nil, sdkerrors. Wrap(sdkerrors.ErrInvalidRequest, "You Have Guessed The Maximum Amount of Times for The Day! Try Again Tomorrow With A New Wordle.")
    }
    currentCount, err := strconv. Vuelva a intentarlo mañana con una nueva Wordle.")
    Atoi(guess.Count)
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
  var newGuess = types. Guess{
    Index:     currentTimeGuesserHashString,
    Submitter: msg. Creator,
    Word:      submittedSolutionHashString,
    Count:     strconv. Itoa(count),
  }
  // Remove Current Guess Entry to be Updated With New Entry
  k. RemoveGuess(ctx, currentTimeGuesserHashString)
  // Add New Guess Entry
  k. SetGuess(ctx, newGuess)
  // Setup Reward 
  reward := sdk. Coins{sdk.NewInt64Coin("WORDLE", 100)}
  if !(wordle.Word == submittedSolutionHashString) {
    return &types.MsgSubmitGuessResponse{Title: "Wrong Answer", Body: "Your Guess Was Wrong. Try Again"}, nil
  } else {
    // If Submitter Guesses Correctly
    guesserAddress, _ := sdk. AccAddressFromBech32(msg.Creator)
    moduleAcct := sdk. AccAddress(crypto.AddressHash([]byte(types.ModuleName)))
    // Send Reward
    k.bankKeeper.SendCoins(ctx, guesserAddress, moduleAcct, reward) 
    return &types.MsgSubmitGuessResponse{Title: "Correct", Body: "You Guessed The Wordle Correctly!"}, nil
  }
}
```

En el código anterior, estamos haciendo las siguientes cosas:

* Aquí estamos volviendo a verificar la palabra para asegurarnos que son 5 caracteres y solo caracteres alfabéticos usados. que puede ser refactorizado en el futuro o comprobado dentro de los comandos CLI.
* Entonces obtenemos la Wordle del día obteniendo la cadena hash de el día actual.
* A continuación crearemos una cadena de hash del día actual y del remitente. Esto nos permite crear un tipo de Guess con un índice que utiliza el día actual y la dirección del envío. Esto nos ayuda cuando nos enfrentamos a un nuevo día y una dirección quiere adivinar la nueva palabra del día. La configuración del índice asegura que pueden continuar adivinando una nueva palabra cada día hasta el máximo de 6 intentos por día.
* Luego verificamos si ese tipo de adivinación para el Enviador de la palabra de hoy llegó a 6 cuentas. Si no lo ha hecho, incrementamos el recuento. Entonces comprobamos si la conjetura es correcta. Almacenamos el tipo de adivinación con el recuento actualizado al estado.

## Archivo Protobuf

  Para que esto funcione, es necesario modificar algunos archivos.

El primero es `proto/wordle/tx.proto`.

Dentro de este archivo, rellena el vacío `MsgSubmitGuessResponse` con el siguiente código:

```go
message MsgSubmitGuessResponse {
  string title = 1;
  string body = 2;
}
```

El siguiente archivo es `x/wordle/types/expected_keepers.go`

Aquí tenemos que añadir el método SendCoins a la interfaz de BankKeeper para poder enviar la recompensa al cliente correcto.

```go
type BankKeeper interface {
  SendCoins(ctx sdk.Context, fromAddr sdk.AccAddress, toAddr sdk.AccAddress, amt sdk.Coins) error
}
```

¡Con eso, implementamos todas nuestras funciones de Keeper! Tiempo para compilar la blockchain y sacarla para una unidad de prueba.
