---
sidebar_label: Ejecuta un Light Node
---

# 🪶 Ejecutar un DA Light Node de Celestia

Se requiere un Light Node de Celestia en Mamaki Testnet para completar este tutorial. Ejecuta el siguiente comando para instalar el Celestia-Node:

<!-- markdownlint-disable MD010 -->
```bash
cd && rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node
git checkout tags/v0.3.0-rc2
make install
```
<!-- markdownlint-enable MD010 -->

![1.png](/img/gm/1.png)

Dentro del repositorio de celestia-node se encuentra una utilidad llamada `cel-key` que usa la utilidad key proporcionada por Cosmos-SDK bajo el capó. La utilidad puede ser usada para `add`, `delete`, y administrar las claves para cualquier nodo DA tipo `(bridge || full || light)`, o simplemente claves en general.

## 🗝 Crear una clave

Crea tu clave para el nodo:

```bash
make cel-key
```

Verifica la versión de tu Celestia-Node con el comando `celestia version`, debería ser `v0.3.0-rc2`:

```bash
celestia version
```

```bash
# OUTPUT

#Semantic version: v0.3.0-rc2
#Commit: 89892d8b96660e334741987d84546c36f0996fbe
#Build Date: Fri Oct  7 01:08:14 UTC 2022
#System version: amd64/linux
#Golang version: go1.18.2
```

## 🟢 Inicializar el light node

Ahora estamos listos para inicializar el Light Node de Celestia. Puedes hacerlo ejecutando:

```bash
celestia light init
```

Consulta la dirección de nuestra clave usando `cel-key`:

```bash
./cel-key list --node.type light --keyring-backend test
```

![2.png](/img/gm/2.png)

## 🚰 Visita el Faucet

Usa el canal `#mamaki-faucet` en el Discord de Celestia para solicitar tokens de testnet:

```bash
$request <Wallet-Address>
```

Iniciar el Light node de Celestia con una conexión a un Core Endpoint público:

<!-- markdownlint-disable MD013 -->
```bash
celestia light start --core.grpc https://rpc-mamaki.pops.one:9090 --keyring.accname my_celes_key
```
<!-- markdownlint-enable MD013 -->

![3.png](/img/gm/3.png)

En otra ventana de terminal, comprueba el saldo de nuestra visita al faucet:

```bash
curl -X GET http://localhost:26658/balance
```

Tu respuesta debería verse así, denominada en `utia` en formato JSON.

```bash
{"denom":"utia","amount":"100000000"}
```

Ahora que estamos preparados con Go e Ignite CLI instalado, y nuestro Light Node de Celestia funcionando en nuestra máquina, estamos listos para construir, probar y lanzar nuestro propio rollup soberano.
