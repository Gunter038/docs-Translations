- - -
sidebar_label : Keys
- - -

# Utilizando la utilidad de cel-key

Dentro del repositorio de celestia-node se encuentra una utilidad llamada `cel-key` que usa la utilidad key proporcionada por Cosmos-SDK bajo el capó. La utilidad puede ser usada para `añadir`, `borrar`, y administrar las claves para cualquier nodo DA tipo `(bridge || full || light)`, o simplemente claves en general.

## Instalación

Primero necesitas descargar el repositorio `celestia-node`:

```sh
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```

Se puede construir usando cualquiera de los siguientes comandos:

```sh
# Muestra el binario en el directorio de trabajo actual, accesible a través de `./cel-key`
make cel-key
```

o

```sh
# instala el binario en la ruta GOBIN, accesible a través de `cel-key`
make install-key
```

Para el propósito de esta guía, usaremos el comando `make cel-key`.

## Pasos para generar claves de nodos **bridge**

Para generar una clave para un nodo de Celestia bridge, haz lo siguiente:

```sh
./cel-key add <key_name> --keyring-backend test --node.type bridge
```

Esto cargará la clave <key_name> en el directorio del nodo bridge.

## Pasos para generar claves de **full** nodes

Para generar una clave para un full node de Celestia, haz lo siguiente:

```sh
./cel-key add <key_name> --keyring-backend test --node.type full
```

Esto cargará la clave <key_name> en el directorio de full node.

## Pasos para generar claves de **light** nodes

Para generar una clave para un light node de Celestia, haz lo siguiente:

```sh
./cel-key add <key_name> --keyring-backend test --node.type light
```

Esto cargará la clave <key_name> en el directorio del light node.

## Pasos para exportar claves de **light** nodes

Puedes exportar una clave privada desde el conjunto de claves local en formato cifrado y con formato ASCII.

```sh
./cel-key export <key-name> --keyring-backend test --node.type light
```

A continuación, puedes importar tu clave con `celestia-appd`:

```sh
celestia-appd keys import <new-key-name> <key-file-location> 
```
