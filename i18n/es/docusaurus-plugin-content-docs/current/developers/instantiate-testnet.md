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

Una vez que todos los participantes te han enviado sus archivos gentx JSON, extraerás todos esos archivos gentx dentro del siguiente directorio: `$HOME/.celestia-appd/config/gentx` y utilízalos para crear el archivo final `genesis.json`.

Una vez que hayas añadido los archivos gentx de todos los participantes, ejecuta el siguiente comando:

```sh
celestia-appd collect-gentxs
```

Este comando buscará los archivos gentx en este repositorio que debería moverse al siguiente directorio `$HOME/.celestia-app/config/gentx`.

Actualizará el archivo `genesis.json` después en esta ubicación `$HOME/.celestia-app/config/genesis.json` que ahora incluye el gentx de otros participantes.

Entonces deberías compartir este género final `genesis.json` y todos los otros participantes deben añadirlo a su directorio `$HOME/.celestia-app/config`.

Todos deben asegurarse de reemplazar su archivo `genesis.json` existente por este nuevo archivo creado.

### Modifica tu archivo de configuración

Abre el siguiente archivo `$HOME/.celestia-app/config/config.toml` para modificarlo.

Dentro del archivo, añade a los otros participantes modificando la siguiente línea para incluir a otros participantes como pares persistentes:

```text
# Lista de nodos separados por comas para mantener conexiones persistentes a
persistent_peers = "[validator_address]@[ip_address]:[port],[validator_address]@[ip_address]:[port]"
```

Puedes encontrar `validator_address` ejecutando el siguiente comando:

```sh
celestia-appd tendermint show-node-id
```

La salida será la `validator_address` codificada en hex. El puerto predeterminado `` es 26656.

### Instanciar la Red

Puede comenzar a sincronizar su nodo ejecutando el siguiente comando:

```sh
celestia-appd start
```

Now you have a new Celestia Testnet to play around with!
