
---


# NFS (Network File System) in Linux

## 📌 What is NFS?
NFS (Network File System) is a distributed file system protocol that allows a Linux system to **share directories and files over a network**, so remote machines can mount them as if they were local file systems.

It is commonly used in:
- Linux server environments
- DevOps & cloud infrastructure
- Kubernetes shared storage (legacy setups)
- Centralized file sharing

---

## 🏗 NFS Architecture
NFS follows a **client–server model**:

- **NFS Server**  
  Exports (shares) directories to the network

- **NFS Client**  
  Mounts exported directories and accesses them like local files

Communication typically happens over **TCP/UDP port 2049**.

---

## 📦 NFS Versions
| Version | Features |
|------|---------|
| NFSv2 | Older, UDP-based |
| NFSv3 | Better performance, async writes |
| NFSv4 | Stateful, better security, ACLs, firewall-friendly |
| NFSv4.1+ | Parallel NFS (pNFS), high performance |

👉 **NFSv4 is recommended** for modern systems.

---

## 🔧 Installing NFS

### On NFS Server
```bash
sudo apt update
sudo apt install nfs-kernel-server -y
````

### On NFS Client

```bash
sudo apt update
sudo apt install nfs-common -y
```

---

## 📂 NFS Export Directory (Server Side)

### Step 1: Create directory

```bash
sudo mkdir -p /srv/nfs/shared
sudo chown nobody:nogroup /srv/nfs/shared
sudo chmod 777 /srv/nfs/shared
```

### Step 2: Configure exports

Edit:

```bash
sudo nano /etc/exports
```

Example:

```bash
/srv/nfs/shared 192.168.1.0/24(rw,sync,no_subtree_check)
```

Apply changes:

```bash
sudo exportfs -rav
```

Restart service:

```bash
sudo systemctl restart nfs-kernel-server
```

---

## 🔑 Common NFS Export Options

| Option           | Description                     |
| ---------------- | ------------------------------- |
| rw               | Read & write access             |
| ro               | Read-only                       |
| sync             | Writes synced to disk           |
| async            | Faster but less safe            |
| no_root_squash   | Root has full access            |
| root_squash      | Root mapped to nobody (default) |
| no_subtree_check | Improves reliability            |

---

## 💻 Mounting NFS on Client

### Temporary Mount

```bash
sudo mount 192.168.1.10:/srv/nfs/shared /mnt/nfs
```

Verify:

```bash
df -h | grep nfs
```

---

### Permanent Mount (fstab)

Edit:

```bash
sudo nano /etc/fstab
```

Add:

```bash
192.168.1.10:/srv/nfs/shared  /mnt/nfs  nfs  defaults  0  0
```

Mount all:

```bash
sudo mount -a
```

---

## 🔍 Checking NFS Exports

From client:

```bash
showmount -e 192.168.1.10
```

---

## 🔐 NFS Security

* Use **firewall rules** to restrict access
* Prefer **NFSv4**
* Avoid `no_root_squash` unless required
* Use **Kerberos (krb5)** for authentication in production
* Restrict exports to specific IP ranges

---

## 🚀 Performance Tuning (Optional)

Example optimized mount:

```bash
mount -t nfs -o rw,hard,intr,rsize=8192,wsize=8192 192.168.1.10:/srv/nfs/shared /mnt/nfs
```

---

## 📊 Use Cases

* Shared home directories
* CI/CD shared workspace
* Backup storage
* Media storage
* Legacy Kubernetes volumes

---

## ⚠ Common Issues & Troubleshooting

### Permission denied

* Check `/etc/exports`
* Verify `root_squash` setting
* Check directory permissions

### Mount hangs

```bash
systemctl status nfs-kernel-server
```

### Firewall issue

```bash
sudo ufw allow from 192.168.1.0/24 to any port nfs
```

---

## 🆚 NFS vs SMB

| Feature     | NFS              | SMB              |
| ----------- | ---------------- | ---------------- |
| OS          | Linux/Unix       | Windows          |
| Performance | Faster in Linux  | Slower           |
| Security    | Kerberos support | AD integration   |
| Use Case    | Linux servers    | Windows networks |

---

## 🧠 Interview Tips

* NFS uses **port 2049**
* `/etc/exports` defines shared directories
* `showmount -e` lists exports
* NFSv4 is **stateful**
* Root is squashed by default

---

## 📚 References

* `man exports`
* `man mount.nfs`
* Linux NFS Documentation

---

## ✅ Conclusion

NFS is a powerful and widely used file-sharing solution in Linux environments.
With proper configuration and security, it provides reliable shared storage for enterprise workloads.

```

---
