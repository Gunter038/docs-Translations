---
sidebar_label: Configuración
---

# 💻Configuración

- Sistemas operativos: GNU/Linux, macOS o subsistema Windows para Linux (WSL). Recomendado GNU/Linux or macOS.

> Este tutorial fue realizado en un Mac M2 con la versión 12.6 de macOS Monterey.

- [Golang v1.18.2](https://go.dev/)
- [Ignite CLI v0.24.0](https://github.com/ignite/cli/releases/tag/v0.24.0)
- [Homebrew](https://brew.sh/)
- [wget](https://www.gnu.org/software/wget/)
- [jq](https://stedolan.github.io/jq/)
- [Un Light Node de Celestia](https://docs.celestia.org/nodes/light-node/)

## 🏃Instalando Golang

Celestia-App, Celestia-Node, y Cosmos-SDK están escritos en el lenguaje de programación de Golang. Necesitaremos Golang para construirlos y ejecutarlos. La red de pruebas Mamaki de Celestia requiere que Golang v1.18.2 compile y ejecute correctamente.

```bash
cd
ver="1.18.2"
wget "https://golang.org/dl/go$ver.darwin-arm64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.darwin-arm64.tar.gz"
rm "go$ver.darwin-arm64.tar.gz"
```

Añade el directorio `/usr/local/go/bin` a [establece correctamente tus variables $PATH](https://go.dev/doc/gopath_code#GOPATH):

```bash
# Si usas bash
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile

# Si usas zsh
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.zshrc
source $HOME/.zshrc
```

Para comprobar si Go fue instalado correctamente ejecuta el comando:

```bash
go version
```

El resultado debe ser la versión instalada:

```bash
go version go1.18.2 darwin/arm64
```

## 🔥 Instalar Ignite CLI

Primero, necesitarás crear `/usr/local/bin` si aún no lo tienes:

```bash
sudo mkdir -p -m 775 /usr/local/bin
```

Ejecuta este comando en tu terminal para instalar Ignite CLI:

```bash
curl https://get.ignite.com/cli! | bash
```

> ✋ En algunas máquinas, se pueden encontrar errores de permisos como el de abajo. Puedes resolver este error siguiendo la guía [aquí](https://docs.ignite.com/guide/install#write-permission) y abajo.

```bash
# Error
jcs @ ~ % curl https://get.ignite.com/cli! | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3967    0  3967    0     0  38631      0 --:--:-- --:--:-- --:--:-- 41322
Installing ignite v0.24.0.....
######################################################################## 100.0%
**mv: rename ./ignite to /usr/local/bin/ignite: Permission denied**
============
**Error: mv failed**
jcs @ ~ %
```

El siguiente comando debe resolver el error de permisos:

```bash
sudo curl https://get.ignite.com/cli! | sudo bash
```

Una instalación exitosa devolverá algo similar a la siguiente respuesta:

```bash
% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3967    0  3967    0     0   4122      0 --:--:-- --:--:-- --:--:--  4136
Installing ignite v0.22.2.....
######################################################################## 100.0%
Installed at /usr/local/bin/ignite
```

Verifica que has instalado Ignite CLI ejecutando:

```bash
ignite version
```

La respuesta que recibas debería ser algo así:

<!-- markdownlint-disable MD010 -->
<!-- markdownlint-disable MD013 -->
```bash
jcs @ ~ % ignite version
Ignite CLI version: v0.24.0
Ignite CLI build date:  2022-09-12T14:14:32Z
Ignite CLI source hash: 21c6430cfcc17c69885524990c448d4a3f56461c
Your OS:        darwin
Your arch:      arm64
Your go version:    go version go1.18.2 darwin/arm64
Your uname -a:      Darwin Joshs-MacBook-Air.local 21.6.0 Darwin Kernel Version 21.6.0: Sat Jun 18 17:07:28 PDT 2022; root:xnu-8020.140.41~1/RELEASE_ARM64_T8110 arm64
Your cwd:       /Users/joshcs
Is on Gitpod:       false
```
<!-- markdownlint-enable MD013 -->
<!-- markdownlint-enable MD010 -->

## 🍺 Instalar Homebrew

Homebrew nos permitirá instalar dependencias para nuestro Mac:

```jsx
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Asegúrate de ejecutar los comandos similares a la salida de abajo después de que la instalación haya sido exitosa:

```jsx
==> Next steps:
- Run these three commands in your terminal to add Homebrew to your PATH:
    echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/joshstein/.zprofile
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/joshstein/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
```

## 🏃 Instalar wget y jq

wget es un recuperador de archivos de Internet y jq es un procesador de JSON flexible y ligero de línea de comandos.

```bash
brew install wget && brew install jq
```
