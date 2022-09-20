- - -
sidebar_label : SystemD
- - -

# 通过SystemD设置您的节点为后台进程

SystemD是一个后台进程管理服务，能方便地使应用程序运行为后台进程。

## 共识节点

如果您正在运行验证者或者说参与共识的全节点，以下是将`celestia-appd`设置为后台进程的步骤。

### 通过SystemD启动Celestia-Appd

SystemD是一个后台进程管理服务，能方便地使应用程序运行为后台进程。

创建Celestia-Appd的systemd文件：

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

如果文件被成功创建，您将能够看到它的内容：

```sh
cat /etc/systemd/system/celestia-appd.service
```

启用并启动celestia-appd后台进程：

```sh
systemctl enable celestia-appd
systemctl start celestia-appd
```

检查后台进程是否已正确启动：

```sh
systemctl status celestia-appd
```

实时查看后台进程日志：

```sh
journalctl -u celestia-appd.service -f
```

在继续之前，请检查您的节点是否已完成同步：

```sh
curl -s localhost:26657/status | jq .result | jq .sync_info
```

请确保您看到`"catching_up": false`, 否则请等待它运行，直至达到同步状态。

## 数据可用节点

### Celestia存储全节点

创建Celestia存储全节点systemd文件：

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

如果文件被成功创建，您将能够看到它的内容：

```sh
cat /etc/systemd/system/celestia-full.service
```

启用并启动全节点的后台进程：

```sh
systemctl enable celestia-full
systemctl start celestia-full && journalctl -u \
celestia-full.service -f
```

您应该能看到存储全节点整个同步过程的日志。

### Celestia桥接节点

创建桥接节点的systemd文件：

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

如果文件被成功创建，您将能够看到它的内容：

```sh
cat /etc/systemd/system/celestia-bridge.service
```

启用并启动桥接节点后台进程：

```sh
systemctl enable celestia-bridge
systemctl start celestia-bridge && journalctl -u \
celestia-bridge.service -f
```

现在，Celestia桥接节点将开始，从Celestia应用程序同步区块头，并存储区块。

> 注意：我们可以从Celestia桥接节点看到`multiaddress`。 Note: At startup, we can see the `multiaddress` from Celestia Bridge Node. This is **needed for future Light Node** connections and communication between Celestia Bridge Nodes

示例：

```sh
NODE_IP=<ip-address>
/ip4/$NODE_IP/tcp/2121/p2p/12D3KooWD5wCBJXKQuDjhXFjTFMrZoysGVLtVht5hMoVbSLCbV22
```

您应该能看到桥接节点整个同步过程的日志。

### Celestia轻节点

把轻节点作为后台进程启动

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

如果文件被成功创建，您将能够看到它的内容：

```sh
cat /etc/systemd/system/celestia-lightd.service
```

启用并启动轻节点的后台进程：

```sh
systemctl enable celestia-lightd
systemctl start celestia-lightd
```

检查后台进程是否已正确启动：

```sh
systemctl status celestia-lightd
```

实时查看后台进程日志：

```sh
journalctl -u celestia-lightd.service -f
```

现在，Celestia轻节点将开始同步区块头。 Now, the Celestia Light Node will start syncing headers. After sync is finished, Light Node will do Data Availability Sampling (DAS) from the Bridge Node.
