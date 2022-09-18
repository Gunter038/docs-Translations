- - -
sidebar_label : Guía de Config.toml
- - -

# Desglose de Config.toml

- [Desglose de Config.toml](#configtoml-breakdown)
  - [Pre-Requisitos](#pre-requisites)
  - [Entendiendo config.toml](#understanding-configtoml)
    - [[Core]](#core)
    - [[P2P]](#p2p)
      - [Bootstrap](#bootstrap)
      - [Mutual Peers](#mutual-peers)
    - [[Services]](#services)
      - [TrustedHash y Trusted Peer](#trustedhash-and-trustedpeer)

## Pre-requisitos

Por favor, asegúrese de que ha instalado e inicializado el nodo celestia

## Entendiendo config.toml

Después de la inicialización, para cualquier tipo de nodo, encontrarás un `config.toml` en la siguiente ruta (ubicación predeterminada):

- `$HOME/.celestia-bridge/config.toml` para nodo bridge
- `$HOME/.celestia-bridge/config.toml` para light node

Desglosemos algunas de las secciones más utilizadas.

### [Core]

Esta sección es necesaria para el Nodo Bridge de Celestia. Por defecto, `Remote = false`. Aún para devnet, vamos a a usar la opción de remote core y esto también puede establecerse por la bandera de línea de comandos `--core.remote`.

### [P2P]

#### Bootstrap

Los Bootstrappers ayudan a los nuevos nodos a encontrar los pares más rápido en la red. Por defecto, el `Bootstrapper = false` y el `BootstrapPeers` está vacío. Si deseas que tu nodo sea un Bootstrapper, entonces activa `Bootstrapper = true`. `BootstrapPeers` ya son proporcionados por defecto durante la inicialización. Si quieres añadir el tuyo manualmente, necesitas proporcionar las multidirecciones de los peers.

#### Mutual Peers

El propósito de esta configuración es establecer una comunicación bidireccional. Este es generalmente el caso de Celestia Bridge Nodes. Además, necesitas cambiar el campo `PeerExchange` de false a true.

### [Services]

#### TrustedHash y TrustedPeer

`TrustedHash` es necesario para inicializar correctamente un Nodo Celestia Bridge con un `Remoto` Nodo Celestia Core, ya ejecutado. El Nodo Light Celestia tomará un hash de génesis como el de confianza, si no se proporciona ningún hash manualmente durante la fase de inicialización.

`TrustedPeers` es el array de peers de Celestia Bridge Nodes en el que Light Node confía. Por defecto, los peers de bootstrap se convierten en pares de confianza para Celestia Light Nodes si un usuario no está configurando los parámetros de confianza en el archivo de configuración.

Cualquier Nodo Bridge de Celestia puede ser un par de confianza para el Light Node. Sin embargo, el nodo Light por diseño no puede ser un par de confianza para otro nodo Light.
