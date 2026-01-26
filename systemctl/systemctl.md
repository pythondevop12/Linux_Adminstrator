
---

# Linux `systemctl` Master Guide

This document provides an overview and quick-reference guide for `systemctl`, the central tool for controlling the **systemd** system and service manager.

## 1. Introduction to systemd

`systemd` is the init system used by most modern Linux distributions (Ubuntu, Fedora, CentOS, Debian) to bootstrap the user space and manage processes. `systemctl` is the command-line utility used to interact with it.

---

## 2. Basic Service Management

The most common use for `systemctl` is managing services (units ending in `.service`).

| Action | Command | Description |
| --- | --- | --- |
| **Start** | `sudo systemctl start <service>` | Starts a service immediately. |
| **Stop** | `sudo systemctl stop <service>` | Stops a running service. |
| **Restart** | `sudo systemctl restart <service>` | Stops and then starts a service. |
| **Reload** | `sudo systemctl reload <service>` | Reloads configuration without dropping connections. |
| **Status** | `systemctl status <service>` | Shows if the service is running and recent logs. |

---

## 3. Boot Management (Enable/Disable)

Managing whether a service starts automatically when the computer boots up.

* **Enable:** `sudo systemctl enable <service>`
* *Creates a symbolic link to ensure the service starts on boot.*


* **Disable:** `sudo systemctl disable <service>`
* *Removes the symbolic link; the service will not start on boot.*


* **Check Status:** `systemctl is-enabled <service>`

---

## 4. System States and Power Management

`systemctl` also handles the state of the entire machine.

* **Reboot:** `systemctl reboot`
* **Power Off:** `systemctl poweroff`
* **Suspend:** `systemctl suspend`
* **Rescue Mode:** `systemctl rescue`

---

## 5. Introspection and Monitoring

How to find what is running on your system.

* **List all active units:**
```bash
systemctl list-units

```


* **List all installed unit files (including disabled ones):**
```bash
systemctl list-unit-files

```


* **List failed units:**
```bash
systemctl --failed

```



---

## 6. Advanced Troubleshooting

When a service won't start, use these commands to find out why.

### Viewing Logs with journalctl

Systemd keeps its own logs. To see logs for a specific service:

```bash
journalctl -u <service-name>.service -f

```

*(The `-f` flag follows the log in real-time.)*

### Analyzing Boot Performance

If your Linux machine is booting slowly, you can find the culprit:

```bash
systemd-analyze blame

```

---

## 7. Creating a Custom Service

To run your own script as a service, create a file at `/etc/systemd/system/my-app.service`:

```ini
[Unit]
Description=My Custom Python App
After=network.target

[Service]
ExecStart=/usr/bin/python3 /home/user/app.py
Restart=always
User=nobody

[Install]
WantedBy=multi-user.target

```

**To apply changes:**

1. `sudo systemctl daemon-reload`
2. `sudo systemctl enable my-app`
3. `sudo systemctl start my-app`

---
