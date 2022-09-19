- - -
sidebar_label : Crear una Testnet de Celestia
- - -

# Guía de Instanciación de la Red Celestia App

Esta guía es para ayudar a instanciar una nueva testnet y seguir los pasos correctos para hacerlo con Celestia-App. Solo debes seguir esta guía si quieres experimentar con tu propia testnet Celestia o si quieres probar nuevas características para construir como desarrollador core.

## Requisitos de hardware

Puedes seguir los requisitos de hardware [aquí](../nodes/validator-node.md#hardware-requirements).

## Actualizando las dependencias

Puede configurar las dependencias siguiendo la guía [aquí](./environment.md).

## Instalación de Celestia App

Puedes instalar Celestia App siguiendo la guía [aquí](./celestia-app.md).

## Comparte una testnet Celestia

Si quieres compartir una testnet rápida con tus amigos, puedes seguir estos pasos. A menos que se indique lo contrario, todos los que quieran participar en esta testnet deben hacer cada paso.

### Opcional: Restablecer Directorio de Trabajo

Si ya has iniciado un directorio de trabajo para `celestia-appd` en el pasado, debes limpiar antes de reinicializar un nuevo directorio. Puedes encontrar la dirección ejecutando el siguiente comando:

```sh
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app
```

### Inicializar un Directorio de Trabajo

Ejecuta el siguiente comando:

```sh
VALIDATOR_NAME=validator1
CHAIN_ID=testnet
celestia-appd init $VALIDATOR_NAME --chain-id $CHAIN_ID
```

* El valor que usaremos para `$VALIDATOR_NAME` es `validator1` pero deberías elegir tu propio nombre de nodo.
* El valor que usaremos para `$CHAIN_ID` es `testnet`. El `$CHAIN_ID` debe permanecer igual para todos los participantes en esta red.

### Crear nueva clave

Ejecuta el siguiente comando:

```sh
KEY_NAME=validator
celestia-appd keys add $KEY_NAME --keyring-backend test
```

Esto creará una nueva clave, con un nombre de su elección. Guarda la salida de este comando en algún lugar; necesitarás la dirección generada aquí más tarde. Aquí establecemos el valor de nuestra clave `$KEY_NAME` a `validador` para demostración.

### Añadir un nombre clave de la cuenta Génesis

Ejecuta el siguiente comando:

```sh
CELES_AMOUNT="10000000000000000000000000utia"
celestia-appd add-genesis-account $KEY_NAME $CELES_AMOUNT --keyring-backend test
```

Here `$VALIDATOR_NAME` is the same key name as before; and `$AMOUNT` is something like `10000000000000000000000000utia`.

### Opcional: Añadir otros validadores

Si otros participantes en tu testnet también quieren ser validadores, repite el comando anterior con la cantidad específica para sus claves públicas.

Una vez que se agregan todos los validadores, se crea el archivo `genesis.json`. Necesitas para compartirlo con todos los demás validadores de tu red de pruebas para que todos puedan continuar con el siguiente paso.

Puede encontrar el `genesis.json` en `$HOME/.celestia-app/config/genesis.json`

### Crear la transacción Génesis para una nueva cadena

Ejecuta el siguiente comando:

```sh
STAKING_AMOUNT=1000000000utia
celestia-appd gentx $KEY_NAME $STAKING_AMOUNT --chain-id $CHAIN_ID \
  --keyring-backend test
```

Esto creará la transacción génesis para su nueva cadena. Aquí `$STAKING_AMOUNT` debe ser al menos `1000000000utia`. Si proporcionas demasiado o muy poco, encontrarás un error al iniciar tu nodo.

Encontrarás el archivo JSON gentx generado dentro de `$HOME/.celestia-app/config/gentx/gentx-$KEY_NAME.json`

> Nota: Si tienes otros validadores en tu red, necesitan también ejecutar el comando anterior con el archivo `genesis.json` que compartiste con en el paso anterior.

### Creando el archivo Génesis JSON

Once all participants have submitted their gentx JSON files to you, you will pull all those gentx files inside the following directory: `$HOME/.celestia-appd/config/gentx` and use them to create the final `genesis.json` file.

Once you added the gentx files of all the particpants, run the following command:

```sh
celestia-appd collect-gentxs
```

This command will look for the gentx files in this repo which should be moved to the following directory `$HOME/.celestia-app/config/gentx`.

It will update the `genesis.json` file after in this location `$HOME/.celestia-app/config/genesis.json` which now includes the gentx of other participants.

You should then share this final `genesis.json` file with all the other particpants who must add it to their `$HOME/.celestia-app/config` directory.

Everyone must ensure that they replace their existing `genesis.json` file with this new one created.

### Modify Your Config File

Open the following file `$HOME/.celestia-app/config/config.toml` to modify it.

Inside the file, add the other participants by modifying the following line to include other participants as persistent peers:

```text
# Comma separated list of nodes to keep persistent connections to
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

You can find `validator_address` by running the following command:

```sh
celestia-appd tendermint show-node-id
```

The output will be the hex-encoded `validator_address`. The default `port` is 26656.

### Instantiate the Network

You can start your node by running the following command:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
