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

Esto compilará el código de la blockchain que acabas de escribir y también creará un archivo génesis y algunas cuentas para que utilices. Una vez que el registro muestra algo como el siguiente log de la salida:

```sh
root@yaz-workshop:~/wordle# ignite chain serve
Cosmos SDK's version is: stargate - v0.45.5
📦 Installing dependencies...
🛠️  Building the blockchain...
💿 Initializing the app...
🙂 Created account "alice" with address "cosmos1skalxj42asjhc7dde3lzzawnksnztqmgy6sned" with mnemonic: "exact arrive betray hawk trim surround exhibit host vibrant sting range robot luxury vague manage settle slide town bread adult pact scene journey elite"
🙂 Created account "bob" with address "cosmos1xe3l8z634frp0ry6qlmzs5vr85x6gcty7tmf0n" with mnemonic: "wisdom jelly fine boat series time panel real world purchase age area coach eager spot fiber slide apology near endorse flight panel ready torch"
🌍 Tendermint node: http://0.0.0.0:26657
🌍 Blockchain API: http://0.0.0.0:1317
🌍 Token faucet: http://0.0.0.0:4500
```

Aquí el comando creó un binario llamado `wordled` y las direcciones `alice` y `bob`, junto con un faucet y API. Estás listo para salir del programa con CTRL-C. La razón de ello es que ejecutaremos el binario `wordled` por separado con parámetros Rollmint añadidos.

Puedes iniciar la cadena con configuraciones optimizadas ejecutando lo siguiente:

```sh
wordled start --rollmint.aggregator true --rollmint.da_layer celestia --rollmint.da_config='{"base_url":"http://XXX.XXX.XXX.XXX:26658","timeout":60000000000,"gas_limit":6000000}' --rollmint.namespace_id 000000000000FFFF --rollmint.da_start_height XXXXX
```

Ten en cuenta que:

> NOTA: En el comando anterior, necesitas pasar una dirección IP del nodo Celestia al `base_url` que tiene una cuenta con los tokens Arabica Devnet. Sigue el tutorial para configurar un Light Node de Celestia y crear una wallet con dinero de la faucet de testnet [aquí](./node-tutorial.md) en la sección de Celestia Node.

También ten en cuenta que:

> IMPORTANTE: Además, en el comando anterior, debes especificar el último Height del bloque en Arabica Devnet para `da_height`. Puedes encontrar el número del último bloque en el explorador [aquí](https://explorer.celestia.observer/arabica). También, para el parámetro `--rollmint.namespace_id`, puedes generar un ID de Espacio de Nombres aleatorio usando el playground [aquí](https://go.dev/play/p/7ltvaj8lhRl)

En otra ventana, ejecuta lo siguiente para enviar una Wordle:

```sh
wordled tx wordle submit-wordle giant --from alice --keyring-backend test --chain-id wordle -b async -y
```

> NOTA: Estamos enviando una transacción de forma asincrónica debido a evitar cualquier error de tiempo de espera. Con Rollmint como sustituto de Tendermint, necesitamos esperar a la red de disponibilidad de datos de Celestia para asegurar que un bloque fue incluido desde Wordle, antes de pasar al siguiente bloque. Actualmente, en Rollmint, el único agregador no se está moviendo hacia adelante con el siguiente bloque de producción siempre y cuando esté tratando de enviar el bloque actual a la red DA. En el futuro, con la selección de líderes, la producción de bloques y la lógica de sincronización mejorará espectacularmente.

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

Después de confirmar la transacción, consulta la `txhash` dada la misma forma en la que lo hiciste. Verás que la respuesta muestra un Invalid Error porque has enviado enteros.

Ahora intenta:

```sh
wordled tx wordle submit-guess ABCDEFG --from alice --keyring-backend test --chain-id wordle -b async -y
```

Después de confirmar la transacción, consulta el `txhash` proporcionado en la misma forma en la que lo hiciste. Verás que la respuesta muestra un error inválido porque has enviado una palabra de más de 5 caracteres.

Ahora intenta enviar otra palabra aunque la anterior ya haya sido enviada

```sh
wordled tx wordle submit-wordle meter --from bob --keyring-backend test --chain-id wordle -b async -y
```

Después de confirmar la transacción, consulta el `txhash` de la misma forma en la que lo hiciste. Obtendrás un error sobre que una palabra ya ha sido enviada durante el día.

Ahora intentemos adivinar una palabra de cinco letras:

```sh
wordled tx wordle submit-guess least --from bob --keyring-backend test --chain-id wordle -b async -y
```

Después de confirmar la transacción, consulta el `txhash` de la misma forma en la que lo hiciste. Dado que no adivinaste la palabra correcta, incrementará el número de conjeturas para la cuenta de Bob.

Podemos verificar esto consultando la lista:

```sh
wordled q wordle list-guess --output json
```

Esto produce todos los objetos Guess enviados hasta ahora, siendo el índice la fecha de hoy y la dirección del envío.

Con eso, implementamos un ejemplo básico de Wordle usando Cosmos-SDK e Ignite y Optimint. Lee cómo puedes extender la base de código.

## Ampliando en el futuro

Puedes extender el código base y mejorar este tutorial revisando el repositorio [aquí](https://github.com/celestiaorg/wordle).

Hay muchas maneras en que esta base de código se puede ampliar:

1. Puedes mejorar los mensajes cuando adivinas la palabra correcta.
2. Puedes hacer hash de la palabra antes de enviarla a la cadena, asegúrate de que el hashing sea local para que no sea revelado a través de front-running por otros que supervisan la cadena de texto plano cuando se envía en cadena.
3. Puedes mejorar la interfaz de usuario en la terminal usando una interfaz agradable para Wordle. Algunos ejemplos son [aquí](https://github.com/nimblebun/wordle-cli).
4. Puedes mejorar la fecha actual para vincularse a una zona horaria específica.
5. Puedes crear un bot que envíe una palabra todos los días a un momento determinado.
6. Puedes crear un front-end vue.js con Ignite usando repositorios de código abierto [aquí](https://github.com/yyx990803/vue-wordle) y [aquí](https://github.com/xudafeng/wordle).
