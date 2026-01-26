
---

# Networking Troubleshooting with `netstat`

The `netstat` (network statistics) command is a powerful CLI tool used for monitoring incoming and outgoing network traffic and viewing routing tables, interface statistics, and multicast memberships.

### Key Use Cases

* **Identifying open ports:** Check which services are listening for connections.
* **Troubleshooting connectivity:** Verify if a connection is established, stuck in `TIME_WAIT`, or failing.
* **Performance Monitoring:** Analyze network interface statistics to find packet drops.
* **Process Mapping:** Find out which specific Process ID (PID) is using a port.

---

## 1. Essential Command Flags

| Flag | Description |
| --- | --- |
| `-a` | Show **all** active ports (listening and non-listening). |
| `-t` | Display **TCP** connections. |
| `-u` | Display **UDP** connections. |
| `-l` | Show only **listening** sockets. |
| `-n` | Show **numerical** addresses (skips DNS lookup for speed). |
| `-p` | Show the **PID/Program name** owning the socket (requires `sudo`). |
| `-r` | Display the kernel **routing table**. |
| `-i` | Display **interface** statistics (MTU, RX/TX). |

---

## 2. Common Cheat Sheet Commands

### Find what is listening on a specific port

Useful when a service fails to start because the "address is already in use."

```bash
# Example: Check port 80 or 443
sudo netstat -tulpn | grep :80

```

### List all active TCP connections

```bash
netstat -at

```

### View Routing Table

Equivalent to the `route` command.

```bash
netstat -nr

```

---

## 3. Troubleshooting Scenarios

### Scenario A: Service Won't Start (Port Conflict)

If you are trying to deploy a web server and it crashes, check if another process has claimed the port:

1. Run `sudo netstat -lntp`.
2. Look for the port number in the "Local Address" column.
3. Identify the PID and use `ps -ef | grep <PID>` to see what it is.

### Scenario B: High Latency or Connection Drops

If a server feels slow, check the interface for errors:

```bash
netstat -i

```

* **RX-ERR / TX-ERR:** If these numbers are high, you may have hardware issues or cable problems.
* **RX-DRP / TX-DRP:** High drop counts often indicate the CPU cannot process packets fast enough or the kernel buffer is full.

### Scenario C: Zombie Connections (TIME_WAIT)

If your application is running out of available sockets, it might be due to too many connections in a `TIME_WAIT` state:

```bash
netstat -nat | grep TIME_WAIT | wc -l

```

* **Fix:** Tune your kernel parameters (e.g., `sysctl net.ipv4.tcp_tw_reuse`) or check why the application isn't closing connections properly.

---

