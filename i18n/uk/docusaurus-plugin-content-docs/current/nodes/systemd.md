- - -
бічна панель: SystemD
- - -

# Налаштування вашого вузла як фонового процесу SystemD

SystemD — сервіс, що допомагає запускати програми як фонові процеси.

## Ноди консенсусу

Якщо ви використовуєте ноду валідатора або повну ноду консенсусу, ось кроки для налаштування `celestia-appd` як фонового процесу.

### Запустіть celestia-app за допомогою SystemD

SystemD — сервіс, що допомагає запускати програми як фонові процеси.

Створіть файл systemd Celestia-App:

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

Якщо файл було створено успішно, ви зможете побачити його зміст:

```sh
cat /etc/systemd/system/celestia-appd.service
```

Увімкніть і запустіть daemon celestia-appd:

```sh
systemctl enable celestia-appd
systemctl start celestia-appd
```

Перевірте, чи правильно запущено daemon:

```sh
systemctl status celestia-appd
```

Перевірте журнали daemon в реальному часі:

```sh
journalctl -u celestia-appd.service -f
```

Щоб перевірити, чи є ваша нода синхронізованою перед пересиланням:

```sh
curl -s localhost:26657/status | jq .result | jq .sync_info
```

Переконайтеся, що у вас є `"catching_up": false`, в інакшому разі залиште його працювати, доки не буде синхронізовано.

## Ноди доступності даних

### Повна нода зберігання Celestia

Створіть системний файл Celestia Full Storage Node:

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

Якщо файл було створено успішно, ви зможете побачити його зміст:

```sh
cat /etc/systemd/system/celestia-full.service
```

Увімкніть і запустіть daemon celestia-full:

```sh
systemctl enable celestia-full
systemctl start celestia-full && journalctl -u \
celestia-full.service -f
```

Ви повинні бачити журнали повної синхронізації ноди зберігання.

### Мостова нода Celestia

Створіть системний файл Celestia Bridge:

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

Якщо файл було створено успішно, ви зможете побачити його зміст:

```sh
cat /etc/systemd/system/celestia-bridge.service
```

Увімкніть і запустіть daemon celestia-bridge:

```sh
systemctl enable celestia-bridge
systemctl start celestia-bridge && journalctl -u \
celestia-bridge.service -f
```

Тепер мостова нода Celestia почне синхронізувати заголовки та зберігати блоки з додатка Celestia.

> Примітка: при запуску, ми можемо побачити `multiaddress` з мостової ноди Celestia. Це **потрібно для майбутніх підключень Спрощеної Ноди** і зв’язку між Мостовими Нодами Celestia

Приклад:

```sh
NODE_IP=<ip-address>
/ip4/$NODE_IP/tcp/2121/p2p/12D3KooWD5wCBJXKQuDjhXFjTFMrZoysGVLtVht5hMoVbSLCbV22
```

Ви повинні бачити журнали синхронізації мостової ноди.

### Спрощена Celestia Нода

Запустіть Спрощену Ноду як демона у фоновому режимі

```sh
sudo tee <<EOF >/dev/null /etc/systemd/system/celestia-lightd.service
[Unit]
Description=celestia-lightd Light Node
After=network-online.target

[Service]
User=$USER
ExecStart=$HOME/go/bin/celestia light start --core.grpc http://<ip>:9090
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

If the file was created successfully you will be able to see its content:

```sh
cat /etc/systemd/system/celestia-lightd.service
```

Увімкніть і запустіть демона celestia-lightd:

```sh
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

Перевірте, чи правильно запущено демона:

```sh
systemctl status celestia-lightd
```

Перевірте логи демона в реальному часі:

```sh
journalctl -u celestia-lightd.service -f
```

Тепер Спрощена Нода почне синхронізувати заголовки. Після завершення синхронізації Light Node виконає вибірку доступності даних (DAS) з Мостової Ноди.
