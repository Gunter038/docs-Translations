---
sidebar_label: Ejecutar la cadena Wordle
---

# Ejecutar la cadena Wordle
<!-- markdownlint-disable MD013 -->

## Construir y ejecutar la cadena Wordle

Abre una nueva terminal y ejecuta los siguientes comandos:

```sh
ignite chain serve 
```

Esto compilará el código de la blockchain que acabas de escribir y también creará un archivo génesis y algunas cuentas para que utilices. Una vez que el registro muestra algo como el siguiente inicia sesión en la salida:

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5...
Instalando dependencias...
🛠️ Construyendo la blockchain...
💿 Inicializando la app...
🙂 Se creó la cuenta "alice" con la dirección "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" con mnemotécnico: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Se creó la cuenta "bob" con la dirección "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" con mnemotécnico: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Nodo Tendermint: http://0.0.0.0:26657
🌍 API de Blockchain: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Aquí el comando creó un binario llamado `wordled` y las direcciones `alice` y `bob`, junto con un faucet y API. Estás listo para salir del programa con CTRL-C. La razón de ello es que ejecutaremos `wordled` por separado con banderas Optimint añadidas.

Puedes iniciar la cadena con configuraciones optimizadas ejecutando lo siguiente:

```sh
wordled start --optimint.aggregator true --optimint.da_layer celestia --optimint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --optimint.namespace_id 000000000000FFFF --optimint.da_start_height XXXXX
```

Ten en cuenta que:

> NOTA: En el comando anterior, necesitas pasar una dirección IP del nodo Celestia al `base_url` que tiene una cuenta con tokens de Mamaki testnet. Sigue el tutorial para configurar un Nodo Celestia Light y crear una wallet con dinero de la faucet de testnet [aquí](./node-tutorial.md) en la sección de Celestia Node.

Ten en cuenta que:

> IMPORTANTE: Además, en el comando anterior, debes especificar la última Block Height en Mamaki Testnet para `da_height`. Puedes encontrar el último número de bloque en el explorador [aquí](https://testnet.mintscan.io/celestia-testnet). También, para la bandera `--optimint.namespace_id`, puedes generar un ID de Espacio de Nombres aleatorio usando el playground [aquí](https://go.dev/play/p/7ltvaj8lhRl)

En otra ventana, ejecuta lo siguiente para enviar una Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async
```

> NOTA: Estamos enviando una transacción de forma asincrónica debido a evitar cualquier error de tiempo de espera. Con Optimint como sustituto de Tendermint, necesitamos esperar a la red de disponibilidad de datos de Celestia para asegurar que un bloque fue incluido de Wordle, antes de pasar al siguiente bloque. Actualmente, en Optimint, el único agregador no se está moviendo hacia adelante con el siguiente bloque de producción siempre y cuando esté tratando de enviar el bloque actual a la red DA. En el futuro, con la selección de líderes, la producción de bloques y la lógica de sincronización mejora espectacularmente.

Esto te pedirá que confirmes la transacción con el siguiente mensaje:

```json
{
  "body":{
    "messages":[
       {
          "@type":"/YazzyYaz.wordle.wordle.MsgSubmitWordle",
          "creator":"cosmos17lk3fgutf00pd5s8zwz5fmefjsdv4wvzyg7d74",
          "word":"giant"
       }
    ],
    "memo":"",
    "timeout_height":"0",
    "extension_options":[
    ],
    "non_critical_extension_options":[
    ]
  },
  "auth_info":{
    "signer_infos":[
    ],
    "fee":{
       "amount":[
       ],
       "gas_limit":"200000",
       "payer":"",
       "granter":""
    }
  },
  "signatures":[
  ]
}
```

Cosmos-SDK te pedirá que confirmes la transacción aquí:

```sh
confirm transaction before signing and broadcasting [y/N]:
```

Confirmar con Y.

Obtendrás una respuesta con un hash de transacción como se muestra aquí:

```sh
code: 19
codespace: sdk
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: ""
timestamp: ""
tx: null
txhash: F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128
```

Ten en cuenta que esto no significa que la transacción haya sido incluida en el bloque todavía. Vamos a consultar el hash de la transacción para comprobar si ha sido incluido en el bloque aún o si hay algún error.

```sh
wordled query tx --type=hash F70C04CE5E1EEC5B7C0E5050B3BEDA39F74C33D73ED504E42A9E317E7D7FE128 --chain-id wordle --output json | jq -r '.raw_log'
```

Esto debería mostrar una salida como la siguiente:

```json
[{"events":[{"type":"message","attributes":[{"key":"action","value":"submit_wordle"
}]}]}]
```

Prueba algunas cosas para divertirte:

```sh
wordled tx wordle submit-guess 12345 --from alice --keyring-backend test --chain-id wordle -b async -y
```

Después de confirmar la transacción, consulta la `txhash` dada la misma forma en la que lo hiciste. Verás que la respuesta muestra un error no válido porque has enviado enteros.

Ahora intenta:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

After confirming the transaction, query the `txhash` given the same way you did above. You will see the response shows an Invalid Error because you submitted a word larger than 5 characters.

Now try to submit another wordle even though one was already submitted

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. You will get an error that a wordle has already been submitted for the day.

Now let’s try to guess a five letter word:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

After submitting the transactions and confirming, query the `txhash` given the same way you did above. Given you didn’t guess the correct word, it will increment the guess count for Bob’s account.

We can verify this by querying the list:

```sh
wordled q wordle list-guess --output json
```

This outputs all Guess objects submitted so far, with the index being today’s date and the address of the submitter.

With that, we implemented a basic example of Wordle using Cosmos-SDK and Ignite and Optimint. Read on to how you can extend the code base.

## Extending in the Future

You can extend the codebase and improve this tutorial by checking out the repository [here](https://github.com/celestiaorg/wordle).

There are many ways this codebase can be extended:

1. You can improve messaging around when you guess the correct word.
2. You can hash the word prior to submitting it to the chain, ensuring the hashing is local so that it’s not revealed via front-running by others monitoring the plaintext string when it’s submitted on-chain.
3. You can improve the UI in terminal using a nice interface for Wordle. Some examples are [here](https://github.com/nimblebun/wordle-cli).
4. You can improve current date to stick to a specific timezone.
5. You can create a bot that submits a wordle every day at a specific time.
6. You can create a vue.js front-end with Ignite using example open-source repositories [here](https://github.com/yyx990803/vue-wordle) and [here](https://github.com/xudafeng/wordle).
