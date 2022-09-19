- - -
sidebar_label : Configurar entorno
- - -

# Entorno de desarrollo

Este tutorial sobrepasará la configuración de tu entorno de desarrollo para ejecutar software Celestia. Este entorno puede utilizarse para el desarrollo, construyendo binarios, y ejecutando nodos.

## Instala las dependencias

Una vez que hayas configurado tu instancia, ssh en la instancia para comenzar a instalar las dependencias necesarias para ejecutar un nodo.

Primero, asegúrate de actualizar el sistema operativo:

```sh
# Si estás usando el gestor de paquetes APT
sudo apt update && sudo apt upgrade -y

# Si estás usando el gestor de paquetes YUM
sudo yum update
```

Estos son paquetes esenciales que son necesarios para ejecutar muchas tareas como descargar archivos, compilar y monitorear el nodo:

<!-- markdownlint-disable MD013 -->
```sh
# Si estás usando el gestor de paquetes APT
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y

# Si estás usando el gestor de paquetes YUM
sudo yum install curl tar wget clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```
<!-- markdownlint-enable MD013 -->

## Instalando Golang

Celestia-app y celestia-node están escritos en [Golang](https://go.dev/) por lo que debemos instalar Golang para compilarlos y ejecutarlos.

```sh
ver="1.18.2"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
```

Ahora necesitamos añadir el directorio `/usr/local/go/bin` a `$PATH`:

```sh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```

Para comprobar si Go fue instalado correctamente:

```sh
go version
```

El resultado debe ser la versión instalada:

```sh
go version go1.18.2 linux/amd64
```
