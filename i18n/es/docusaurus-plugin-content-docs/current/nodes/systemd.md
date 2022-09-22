- - -
sidebar_label : SystemD
- - -

# Configurando tu nodo como un proceso en segundo plano con SystememD

SystemD es un servicio de daemon útil para ejecutar aplicaciones como procesos en segundo plano.

## Nodos de consenso

Si estás ejecutando un nodo completo de validador o consenso, aquí tienes los pasos para configurar `celestia-appd` como un proceso en segundo plano.

### Iniciar celestia-app con SystemD

SystemD es un servicio de daemon útil para ejecutar aplicaciones como procesos en segundo plano.

Crear archivo de sistema Celestia-App:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-appd.service
[Unit]
Description=celestia-appd Cosmos daemon
After=network-online.target
[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia-appd start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096
[Install]
WantedBy=multi-user.target
EOF
```

Si el archivo se ha creado con éxito, podrás ver su contenido:

```sh
cat /etc/systemd/system/celestia-appd.service
```

Activar e iniciar daemon de celestia-appd:

```sh
systemctl enable celestia-appd
systemctl start celestia-appd
```

Comprueba si el demonio se ha iniciado correctamente:

```sh
systemctl status celestia-appd
```

Comprueba los registros de daemon en tiempo real:

```sh
journalctl -u celestia-appd.service -f
```

Para comprobar si tu nodo está sincronizado antes de continuar:

```sh
curl -s localhost:26657/status | jq .result | jq .sync_info
```

Asegúrate de que tienes `"catching_up": false`, de lo contrario déjalo ejecutando hasta que esté sincronizado.

## Nodos de disponibilidad de datos

### Nodo full storage Celestia

Crear archivo de nodo de Full Storage de Celestia:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-full.service
[Unit]
Description=celestia-full Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia full start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Si el archivo se ha creado con éxito, podrás ver su contenido:

```sh
cat /etc/systemd/system/celestia-full.service
```

Activar e iniciar el demonio celestia-full:

```sh
systemctl enable celestia-full
systemctl start celestia-full && journalctl -u \
celestia-full.service -f
```

Deberías estar viendo los registros que aparecen a través de la sincronización de nodos de full storage.

### Nodo bridge de Celestia

Crear el archivo systemd de Celestia Bridge:

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-bridge.service
[Unit]
Description=celestia-bridge Cosmos daemon
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia bridge start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

Si el archivo se ha creado con éxito, podrás ver su contenido:

```sh
cat /etc/systemd/system/celestia-bridge.service
```

Activar e iniciar daemon de celestia-appd:

```sh
systemctl enable celestia-bridge
systemctl start celestia-bridge && journalctl -u \
celestia-bridge.service -f
```

Ahora, el nodo de puente Celestia comenzará a sincronizar cabeceras y bloques de almacenamiento desde la aplicación Celestia.

> Nota: al inicio, podemos ver la `multiaddress` de Celestia Bridge Node. Esto es **necesario para futuras conexiones de Nodo Light** y comunicación entre Nodos Celestia Bridge

Ejemplo:

```sh
NODE_IP=<ip-address>
/ip4/$NODE_IP/tcp/2121/p2p/12D3KooWD5wCBJXKQuDjhXFjTFMrZoysGVLtVht5hMoVbSLCbV22
```

Deberías estar viendo los registros que aparecen a través de la sincronización de nodos de full storage.

### Nodo ligero de Celestia

Inicia el Nodo Light como proceso demonio en segundo plano

<!-- markdownlint-disable MD013 -->
```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
[Unit]
Description=celestia-lightd Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia light start --core.ip <ip-address> --core.grpc.port <port>
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```
<!-- markdownlint-enable MD013 -->

Si el archivo se ha creado con éxito, podrás ver su contenido:

```sh
cat /etc/systemd/system/celestia-lightd.service
```

Activar e iniciar daemon de celestia-appd:

```sh
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

Comprueba si el demonio se ha iniciado correctamente:

```sh
systemctl status celestia-lightd
```

Comprueba los registros de daemon en tiempo real:

```sh
journalctl -u celestia-lightd.service -f
```

Ahora, el Nodo Celestia Light comenzará a sincronizar las cabeceras. Después de que finalice la sincronización, Light Node hará Data Availability Sampling (DAS) del Nodo Bridge.
