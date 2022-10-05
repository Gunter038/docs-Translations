- - -
sidebar_label : Creando una wallet
- - -

# Wallet

## Crear una Wallet

Primero, crea un archivo de configuración CLI de la aplicación:

```sh
celestia-appd config keyring-backend test
```

Puedes elegir el nombre de la wallet que desees. Para nuestro ejemplo usamos "validator" como nombre de la wallet:

```sh
celestia-appd keys add validator
```

Guarda la salida de mnemonic ya que esta es la única manera de recuperar tu wallet de validadores en caso de que la pierdas!

Para comprobar todas tus wallets puedes ejecutar:

```sh
celestia-appd keys list
```

## Deposita fondos en la wallet

Para la dirección pública de celestia, puedes añadir fondos a la wallet previamente creada a través de [Discord](https://discord.gg/celestiacommunity) enviando este mensaje al canal #faucet:

```text
$request celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Espera a ver si obtienes una confirmación de que los tokens han sido enviados con éxito. Para comprobar si los tokens han llegado con éxito a la wallet de destino, ejecute el comando de abajo reemplazando la dirección pública por la tuya propia:

```sh
celestia-appd start
celestia-appd query bank balances celestia1xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```
