- - -
sidebar_label : Instalar el nodo Celestia
- - -

# Nodo Celestia

Este tutorial trata sobre la compilación e instalación de celestia-node. Este tutorial presupone que completaste los pasos para configurar tu propio entorno [aquí](./environment.md).

## Instalar el nodo Celestia

Instala el binario celestia-node ejecutando los siguientes comandos:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Verifica que el binario está funcionando y comprueba la versión con el comando de versión celestia:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```
