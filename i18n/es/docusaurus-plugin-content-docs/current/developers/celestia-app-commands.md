- - -
sidebar_label : Comandos CLI útiles
- - -

# Comandos CLI útiles

Ver todas las opciones:

```console
[command][command]
```

## Creando una wallet

```sh
celestia-appd config keyring-backend test
```

`keyring-backend` configura el backend del almacen, donde se guardan las llaves.

Las opciones son: `os|file|kwallet|pass|test|memory`.

## Gestión de claves

```sh
# listando claves
celestia-appd lista

# añadir teclas
celestia-appd add <KEY_NAME>

# eliminar teclas
celestia-appd delete <KEY_NAME>

# renombrar teclas
celestia-appd keys rename <CURRENT_KEY_NAME> <NEW_KEY_NAME>
```

### Importando y exportando claves

Importa una clave privada cifrada y con clave privada ASCII-armored en la base de claves local.

```sh
celestia-appd keys import <KEY_NAME> <KEY_FILE>
```

Ejemplo de uso:

```sh
celestia-appd keys import amanda ./keyfile.txt
```

Puedes exportar una clave privada desde el conjunto de claves local en formato cifrado y con formato ASCII:

```sh
celestia-appd keys export <KEY_NAME>

# se te pedirá que establezcas una contraseña para la clave privada cifrada:
Enter passphrase to encrypt the exported key:
```

Después de establecer una contraseña, se mostrará la clave cifrada.

## Consultando subcomandos

Utilización:

```sh
celestia-appd query <FLAGS> | <COMMAND>

# alias q
celestia-appd q <FLAGS> | <COMMAND>
```

Ver todas las opciones:

```sh
celestia-appd q --help
```

## Gestión de tokens

Obtener los balances de los tokens:

```sh
celestia-appd q bank balances <ADDRESS> --node <NODE_URI>
```

Ejemplo de uso:

```sh
celestia-appd q bank balances celestia1czpgn3hdh9sodm06d5qk23xzgpq2uyc8ggdqgw \
--node https://rpc-mamaki.pops.one
```

Transferir tokens de una wallet a otra:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
<amount> --node <NODE_URI> --chain-id <CHAIN_ID>
```

Ejemplo de uso:

```sh
celestia-appd tx bank send <FROM_ADDRESS> <TO_ADDRESS> \
19000000utia --node https://rpc-mamaki.pops.one/ --chain-id mamaki
```

Ver todas las opciones:

```sh
celestia-appd tx bank send --help
```
