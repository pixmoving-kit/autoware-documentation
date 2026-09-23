<a id="enable-multicast-on-lo"></a>

# 为 `lo` 启用 `multicast`

<a id="manually-temporary-solution"></a>

## 手动启用（临时方案）

执行以下命令，即可在回环接口上启用组播。

```bash
sudo ip link set lo multicast on
```

!!! warning

    计算机重启后，此设置会恢复。若要永久生效，请按照下文操作。

!!! note

    此处的 `lo` 是回环接口。

    可以使用 `ip link show` 查看接口。

    您可以将 `lo` 替换为希望启用组播的接口。

<a id="on-startup-with-a-service-permanent-solution"></a>

## 通过服务在启动时启用（永久方案）

```bash
sudo nano /etc/systemd/system/multicast-lo.service
```

将以下内容粘贴到文件中：

```service
[Unit]
Description=Enable Multicast on Loopback

[Service]
Type=oneshot
ExecStart=/usr/sbin/ip link set lo multicast on

[Install]
WantedBy=multi-user.target
```

在 nano 中依次按以下按键保存：

1. `Ctrl+X`
2. `Y`
3. `Enter`

```bash
# Make it recognized
sudo systemctl daemon-reload

# Make it run on startup
sudo systemctl enable multicast-lo.service

# Start it now
sudo systemctl start multicast-lo.service
```

<a id="validate"></a>

### 验证

```console
you@pc:~$ sudo systemctl status multicast-lo.service
○ multicast-lo.service - Enable Multicast on Loopback
     Loaded: loaded (/etc/systemd/system/multicast-lo.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Mon 2024-07-08 12:54:17 +03; 4s ago
    Process: 22588 ExecStart=/usr/bin/ip link set lo multicast on (code=exited, status=0/SUCCESS)
   Main PID: 22588 (code=exited, status=0/SUCCESS)
        CPU: 1ms

Tem 08 12:54:17 mfc-leo systemd[1]: Starting Enable Multicast on Loopback...
Tem 08 12:54:17 mfc-leo systemd[1]: multicast-lo.service: Deactivated successfully.
Tem 08 12:54:17 mfc-leo systemd[1]: Finished Enable Multicast on Loopback.
```

```console
you@pc:~$ ip link show lo
1: lo: <LOOPBACK,MULTICAST,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

<a id="uninstalling-the-service"></a>

### 卸载服务

如果需要卸载此服务，可以按照以下步骤操作：

```bash
# Stop the service
sudo systemctl stop multicast-lo.service

# Disable the service from running on startup
sudo systemctl disable multicast-lo.service

# Remove the service file
sudo rm /etc/systemd/system/multicast-lo.service

# Reload systemd to apply the changes
sudo systemctl daemon-reload
```
