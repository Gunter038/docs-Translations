- - -
sidebar_label : Instalar el nodo Celestia
- - -

# Nodo Celestia

Este tutorial trata sobre la compilación e instalación de celestia-node. Este tutorial presupone que completaste los pasos para configurar tu propio entorno [aquí](./environment.md).

## Instalar el nodo Celestia

### Instalación Arabica

Instalar celestia-node para Arabica Devnet significa instalar una versión específica para ser compatible con la red.

Instala el binario celestia-node ejecutando los siguientes comandos:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

Verifica que el binario está funcionando y comprueba la versión con el comando de versión celestia:

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Instalación Mamaki

Instalar celestia-node para Mamaki Testnet significa instalar una versión específica para ser compatible con la red.

Instala celestia-node binario ejecutando los siguientes comandos:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Verificar que el binario está funcionando y comprobar la versión con el comando `celestia
versión`:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## Seleccción de red

Puedes realizar la selección de red en el nodo Celestia entre Arabica y Mamaki. Sin embargo, debes tener en cuenta que las redes funcionan mejor en las versiones de celestia-node mencionadas anteriormente.

```sh
celestia light init
celestia light start --node.network arabica
```
