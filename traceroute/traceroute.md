Got it 👍
Below is a **ready-to-drop `README.md`** in **pure Markdown**, covering **`traceroute` usage + deep troubleshooting**, practical flags, examples, and common failure patterns.
You can paste this directly into your repo.

---

````md
# Linux Traceroute – Complete Guide & Troubleshooting

`traceroute` is a Linux network diagnostic tool used to trace the path packets take from a source machine to a destination host.  
It helps identify **network latency**, **routing loops**, **packet drops**, and **firewall issues**.

---

## Table of Contents
- [What is Traceroute](#what-is-traceroute)
- [How Traceroute Works](#how-traceroute-works)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Common Options](#common-options)
- [Traceroute Using Different Protocols](#traceroute-using-different-protocols)
- [Reading Traceroute Output](#reading-traceroute-output)
- [Troubleshooting Scenarios](#troubleshooting-scenarios)
- [Traceroute vs Tracepath vs MTR](#traceroute-vs-tracepath-vs-mtr)
- [Best Practices](#best-practices)

---

## What is Traceroute

Traceroute identifies:
- Network hops between source and destination
- Latency at each hop
- Where packets are dropped or blocked

Common use cases:
- Debug slow network connections
- Find routing loops
- Identify firewall or ISP issues
- Validate connectivity in cloud environments (AWS, Azure, GCP)

---

## How Traceroute Works

Traceroute works by:
1. Sending packets with increasing **TTL (Time To Live)** values
2. Each router decrements TTL by 1
3. When TTL reaches 0, router sends back **ICMP Time Exceeded**
4. Traceroute records each hop until destination is reached

---

## Installation

### Ubuntu / Debian
```bash
sudo apt update
sudo apt install traceroute
````

### RHEL / CentOS / Amazon Linux

```bash
sudo yum install traceroute
```

### Alpine

```bash
apk add traceroute
```

---

## Basic Usage

```bash
traceroute google.com
```

Using IP address:

```bash
traceroute 8.8.8.8
```

Limit number of hops:

```bash
traceroute -m 10 google.com
```

---

## Common Options

| Option | Description                 |
| ------ | --------------------------- |
| `-n`   | Do not resolve DNS (faster) |
| `-m`   | Max hops                    |
| `-w`   | Timeout per probe           |
| `-q`   | Number of probes per hop    |
| `-I`   | Use ICMP                    |
| `-T`   | Use TCP                     |
| `-p`   | Destination port            |
| `-4`   | Force IPv4                  |
| `-6`   | Force IPv6                  |

Example:

```bash
traceroute -n -w 2 -q 1 google.com
```

---

## Traceroute Using Different Protocols

### 1. ICMP (like ping)

```bash
traceroute -I google.com
```

✔ Useful when UDP is blocked

---

### 2. TCP Traceroute (Very Important for Production)

```bash
traceroute -T -p 443 google.com
```

✔ Best for:

* Firewall debugging
* Load balancers
* Kubernetes / EKS / ALB / NLB
* Corporate networks

---

### 3. UDP (Default)

```bash
traceroute google.com
```

⚠ Often blocked by firewalls

---

## Reading Traceroute Output

Example:

```text
1  192.168.1.1   1.123 ms  1.245 ms  1.201 ms
2  10.10.0.1     5.210 ms  5.311 ms  5.298 ms
3  * * *
4  52.93.112.1   45.112 ms  44.981 ms  45.210 ms
```

### Meaning:

* `* * *` → Packet blocked / no response
* High latency → Congestion or routing issue
* Repeating IPs → Possible routing loop

---

## Troubleshooting Scenarios

### 1. Traceroute Stops Midway (`* * *`)

Possible causes:

* Firewall blocking ICMP/UDP
* Router configured to not respond
* NAT Gateway dropping TTL packets

Fix:

```bash
traceroute -T -p 443 <destination>
```

---

### 2. Traceroute Very Slow

Causes:

* DNS resolution delay
* Network congestion

Fix:

```bash
traceroute -n <destination>
```

---

### 3. Traceroute Works but Application Fails

Reason:

* App port blocked
* Load balancer or security group issue

Check using TCP:

```bash
traceroute -T -p 80 <host>
```

---

### 4. Cloud Environment (AWS / Azure / GCP)

Common issues:

* Security Group blocks ICMP
* NACL blocks ephemeral ports
* Private subnet routing issues

Debug:

```bash
traceroute -T -p 443 <alb-dns-name>
```

---

### 5. Kubernetes / EKS Issues

Problem:

* Pod reachable but service not

From node:

```bash
traceroute -T -p 30000 <node-ip>
```

From pod:

```bash
kubectl exec -it pod -- traceroute service-name
```

---

## Traceroute vs Tracepath vs MTR

| Tool       | Use Case                    |
| ---------- | --------------------------- |
| traceroute | Manual hop-by-hop analysis  |
| tracepath  | No root needed              |
| mtr        | Real-time traceroute + ping |

Install mtr:

```bash
sudo apt install mtr
```

Run:

```bash
mtr google.com
```

---

## Best Practices

✔ Use `-n` to avoid DNS delay
✔ Use `-T -p 443` in production
✔ Run from **source & destination**
✔ Combine with `ping`, `curl`, `telnet`, `nc`
✔ In cloud, always check **Security Group + Route Table + NACL**

---

## Quick Debug Checklist

```bash
ping <host>
traceroute -n <host>
traceroute -T -p 443 <host>
curl -v https://<host>
```

---

## Summary

Traceroute helps you:

* Understand network paths
* Identify latency & packet loss
* Debug firewall & routing issues

For production debugging, **TCP traceroute on port 443 is the most reliable method**.

---

### Author

Amit Kumar
Cloud / DevOps Engineer

```

---

If you want, next I can:
- Add **real AWS / EKS examples**
- Create a **short cheat-sheet**
- Convert this into **GitHub-style docs**
- Add **network diagrams explanation (text-only)**
```
