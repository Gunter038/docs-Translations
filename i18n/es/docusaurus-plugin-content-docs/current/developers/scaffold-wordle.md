---
sidebar_label: Scaffolding (andamiaje) de la cadena
---

# Ignite y el Scaffolding (andamiaje) de la cadena Wordle
<!-- markdownlint-disable MD013 -->

## Ignite

Ignite es una increíble herramienta de CLI para ayudarnos a empezar a construir nuestros propios blockchains para aplicaciones cosmos-sdk. Proporciona muchas herramientas poptentes y scaffoldings para añadir mensajes, tipos, y módulos con una gran cantidad de bibliotecas de cosmos-sdk.

Puedes aprender más sobre CosmWasm [aquí](https://docs.ignite.com/).

Para instalar Ignite, puedes ejecutar este comando en tu terminal:

```sh
curl https://get.ignite.com/cli | bash
sudo mv ignite /usr/local/bin/
```

Esto instala Ignite CLI en tu máquina local. Este tutorial usa un MacOS pero debería funcionar para Windows. Para los usuarios de Windows, consulta la documentación de Ignite sobre la instalación para máquinas Windows.

Ahora, actualiza tu terminal usando `source` o abre una nueva sesión de terminal para que el cambio tenga lugar.

Ejecuta las siguientes instrucciones:

```sh
ignite --help
```

¡Deberías ver una salida de comandos de ayuda que significa que Ignite se ha instalado correctamente!

## Scaffolding (andamiaje) de la cadena Wordle

Ahora, viene la parte divertida, creando una nueva blockchain! Con Ignite, el proceso es bastante fácil y directo.

Ignite CLI viene con varios comandos de scaffolding que están diseñados para hacer el desarrollo más directo creando todo lo que necesitas para construir tu blockchain.

Primero, usaremos Ignite CLI para construir los cimientos de una nueva blockchain de Cosmos SDK. Ignite minimiza la cantidad de código de la blockchain que debes escribir. Si vienes del mundo EVM, piensa en Ignite como una versión Cosmos-SDK de Foundry o Hardhat pero específicamente diseñada para construir blockchains.

Primero ejecutamos el siguiente comando para configurar nuestro proyecto para nuestra nueva blockchain, Wordle.

```sh
ignite scaffold chain github.com/YazzyYaz/wordle --no-module
```

Este comando crea un andamiaje (scaffolds) en un nuevo directorio de cadena llamado `wordle` en tu directorio local desde el cual ejecutaste el comando. Observa que pasamos la bandera `--no-module`, esto es porque estaremos creando el módulo después.

## Directorio Wordle

Ahora, es el momento de introducir el directorio:

```sh
cd wordle
```

En el interior verás varios directorios y arquitectura para tu blockchain cosmos-sdk.

| Directorio/archivo | Finalidad                                                                                                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| app/               | Archivos que conectan juntos la blockchain. El archivo más importante es `app.go` que contiene la definición del tipo de blockchain y las funciones para crearla e inicializarla. |
| cmd/               | El paquete principal responsable del CLI del binario compilado.                                                                                                                   |
| docs/              | Directorio para la documentación del proyecto. Por defecto, se genera una especificación OpenAPI.                                                                                 |
| proto/             | Archivos de buffer de protocolo que describen la estructura de datos.                                                                                                             |
| testutil/          | Funciones auxiliares para pruebas.                                                                                                                                                |
| vue/               | Una plantilla de aplicación web Vue 3.                                                                                                                                            |
| x/                 | Módulos Cosmos SDK y módulos personalizados.                                                                                                                                      |
| config.yml         | Un archivo de configuración para personalizar una cadena en desarrollo.                                                                                                           |
| readme.md          | Un archivo léame para tu proyecto de blockchain específico de aplicación soberana.                                                                                                |

Profundizar más en cada uno de estos elementos está fuera del ámbito de esta guía, pero te animamos a a leer sobre ella [aquí](https://docs. ignite. com/kb).

La mayor parte del trabajo del tutorial se desarrollará dentro del directorio `x`.
