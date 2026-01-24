# Linux Networking Security: `firewalld` Cheat Sheet

This guide breaks down the core concepts and commands for managing `firewalld`, the dynamic firewall manager used in major Linux distributions like RHEL, Fedora, and CentOS.

---

## 1. What is a Firewall?

In the Linux ecosystem, a firewall acts as the software layer between your system and the network. It filters traffic based on a defined set of security rules.

* **The Concept:** Think of it as a kernel-level filter. It inspects every packet. If the packet doesn't match an "Allow" rule, it gets dropped or rejected.
* **Purpose:** To minimize the attack surface of your Linux server by closing unused ports and services.

---

## 2. The `firewalld` Service

`firewalld` is a daemon that provides a high-level interface for managing `nftables` or `iptables`. Its biggest advantage is **dynamic management**, meaning you can update rules without restarting the service or breaking established connections.

### Key Concept: Zones

Zones are essentially "trust levels" for your network interfaces.

* **Public:** Default zone for untrusted networks. Only selected incoming connections are accepted.
* **Home/Internal:** Higher trust level; assumes other computers on the network won't harm you.
* **Drop:** The strictest level. Any incoming packet is dropped with no reply (makes the system "invisible").

---

## 3. Service Management (`systemctl`)

Before running configuration commands, you need to ensure the service is active.

| Action | Command |
| --- | --- |
| **Start Firewall** | `sudo systemctl start firewalld` |
| **Stop Firewall** | `sudo systemctl stop firewalld` |
| **Enable (at Boot)** | `sudo systemctl enable firewalld` |
| **Disable (at Boot)** | `sudo systemctl disable firewalld` |
| **Check Status** | `sudo systemctl status firewalld` |

---

## 4. Viewing Existing Rules

Use `firewall-cmd` to inspect your current configuration.

* **View everything in the default zone:**
```bash
sudo firewall-cmd --list-all

```


* **Check which services are currently permitted:**
```bash
sudo firewall-cmd --list-services

```



---

## 5. Adding & Deleting Rules

Rules can be applied by **Service Name**. Linux keeps a list of these in `/usr/lib/firewalld/services/`.

* **Add Service (Runtime/Temporary):** `sudo firewall-cmd --add-service=http`
* **Add Service (Permanent):**
`sudo firewall-cmd --permanent --add-service=http`
* **Remove Service:**
`sudo firewall-cmd --permanent --remove-service=http`

> **Note:** Any command with `--permanent` requires a reload to apply:
> `sudo firewall-cmd --reload`

---

## 6. Managing Ports directly

If you aren't using a standard service name, you can open specific ports using the protocol (TCP or UDP).

* **Open a Port:**
```bash
sudo firewall-cmd --permanent --add-port=8080/tcp

```


* **Close a Port:**
```bash
sudo firewall-cmd --permanent --remove-port=8080/tcp

```



---

## 7. Blocking Traffic (Rich Rules & Panic)

For more granular control, like blocking specific IP addresses, use **Rich Rules**.

* **Block a specific IP:**
```bash
sudo firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='192.168.1.100' reject"

```


* **Panic Mode:** The "nuclear option" to shut down all network activity.
* **Enable:** `sudo firewall-cmd --panic-on`
* **Disable:** `sudo firewall-cmd --panic-off`



---

## 8. Blocking ICMP (Ping)

Blocking ICMP prevents your Linux machine from responding to `ping` requests, which can help hide it from basic network scans.

* **List all ICMP types:**
`sudo firewall-cmd --get-icmptypes`
* **Block Echo Requests:**
```bash
sudo firewall-cmd --permanent --add-icmp-block=echo-request
sudo firewall-cmd --reload

```



---
