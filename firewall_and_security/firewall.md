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

````md
# How to Check if firewalld Exists and Is Running on Linux

## 1. Check if firewalld Package Is Installed
```bash
rpm -q firewalld
````

* If installed:

```text
firewalld-<version>
```

* If not installed:

```text
package firewalld is not installed
```

> Works on RHEL / CentOS / Rocky / AlmaLinux

---

## 2. Check firewalld Service Status (systemd)

```bash
systemctl status firewalld
```

Possible outputs:

* Active and running → firewalld exists and is enabled
* Unit firewalld.service could not be found → not installed

---

## 3. Check If firewalld Service Exists (Without Starting)

```bash
systemctl list-unit-files | grep firewalld
```

---

## 4. Check firewalld Binary

```bash
which firewall-cmd
```

or

```bash
firewall-cmd --state
```

* Running:

```text
running
```

* Installed but stopped:

```text
not running
```

* Not installed:

```text
command not found
```

---

## 5. Debian / Ubuntu Systems

```bash
dpkg -l | grep firewalld
```

or

```bash
apt list --installed | grep firewalld
```

---

## 6. Check Alternative Firewall (Ubuntu Default)

Ubuntu often uses `ufw` instead of firewalld.

Check UFW:

```bash
ufw status
```

---

## 7. Check iptables / nftables (Low-Level)

```bash
iptables -L
```

or

```bash
nft list ruleset
```

---

## 8. Quick One-Liner (Most Useful)

```bash
systemctl is-active firewalld && systemctl is-enabled firewalld
```

---

## Summary

* firewalld installed → `firewall-cmd` exists
* firewalld running → `firewall-cmd --state`
* firewalld managed by systemd → `systemctl status firewalld`
* Ubuntu usually uses `ufw`, not firewalld

```
```
