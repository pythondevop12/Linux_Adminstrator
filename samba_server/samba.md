
---

# Samba Server in Linux (SMB/CIFS)

## 📌 What is Samba?
Samba is an open-source implementation of the **SMB/CIFS protocol** that allows **file and printer sharing between Linux and Windows systems**.

It enables:
- Linux ↔ Windows file sharing
- Linux ↔ Linux (via SMB)
- Centralized shared storage in mixed OS environments

---

## 🏗 Samba Architecture
Samba follows a **client–server model**:

- **Samba Server**
  - Hosts shared directories
  - Manages authentication & permissions

- **Samba Client**
  - Windows, Linux, or macOS system
  - Accesses shares using SMB protocol

Default ports:
- **TCP 445** (SMB)
- **TCP 139** (NetBIOS)

---

## 📦 Installing Samba

### On Samba Server
```bash
sudo apt update
sudo apt install samba -y
````

Verify installation:

```bash
smbd --version
```

---

### On Samba Client (Linux)

```bash
sudo apt install smbclient cifs-utils -y
```

---

## 📂 Creating a Samba Share (Server Side)

### Step 1: Create shared directory

```bash
sudo mkdir -p /srv/samba/shared
sudo chmod 777 /srv/samba/shared
```

---

### Step 2: Create Samba user

Samba uses **system users + separate SMB passwords**.

```bash
sudo useradd sambauser
sudo smbpasswd -a sambauser
```

Enable user:

```bash
sudo smbpasswd -e sambauser
```

---

### Step 3: Configure Samba

Edit config file:

```bash
sudo nano /etc/samba/smb.conf
```

Add at the bottom:

```ini
[shared]
   path = /srv/samba/shared
   browseable = yes
   writable = yes
   valid users = sambauser
   create mask = 0777
   directory mask = 0777
```

---

### Step 4: Restart Samba

```bash
sudo systemctl restart smbd
sudo systemctl enable smbd
```

---

## 🔍 Checking Samba Configuration

```bash
testparm
```

List shares:

```bash
smbclient -L localhost -U sambauser
```

---

## 💻 Accessing Samba Share

### From Linux Client

```bash
smbclient //192.168.1.10/shared -U sambauser
```

Mount permanently:

```bash
sudo mount -t cifs //192.168.1.10/shared /mnt/samba \
-o username=sambauser,password=PASSWORD
```

---

### Permanent Mount (`/etc/fstab`)

```bash
//192.168.1.10/shared  /mnt/samba  cifs  username=sambauser,password=PASSWORD,_netdev  0  0
```

Mount:

```bash
sudo mount -a
```

---

### From Windows Client

1. Open **File Explorer**
2. Enter:

```
\\192.168.1.10\shared
```

3. Login using:

   * Username: `sambauser`
   * Password: (set via smbpasswd)

---

## 🔑 Common Samba Configuration Options

| Option         | Description        |
| -------------- | ------------------ |
| browseable     | Visible in network |
| writable       | Read/write access  |
| read only      | Read-only share    |
| valid users    | Allowed users      |
| guest ok       | Anonymous access   |
| create mask    | File permissions   |
| directory mask | Folder permissions |

---

## 🔐 Samba Security Best Practices

* Disable guest access unless required
* Restrict users with `valid users`
* Use firewall rules
* Avoid sharing `/home` directly
* Use strong SMB passwords

Firewall:

```bash
sudo ufw allow samba
```

---

## 📊 Use Cases

* Linux ↔ Windows file sharing
* Office file servers
* Media sharing
* Backup storage
* Mixed OS environments

---

## ⚠ Common Issues & Troubleshooting

### Authentication Failed

```bash
sudo smbpasswd -a username
```

### Permission Denied

```bash
sudo chown -R sambauser:sambauser /srv/samba/shared
sudo chmod -R 775 /srv/samba/shared
```

### Service not running

```bash
systemctl status smbd
```

---

## 🆚 Samba vs NFS

| Feature         | Samba            | NFS           |
| --------------- | ---------------- | ------------- |
| Best for        | Windows networks | Linux/Unix    |
| Protocol        | SMB/CIFS         | NFS           |
| Authentication  | User-based       | Host/IP based |
| Performance     | Moderate         | High          |
| Windows Support | Native           | Limited       |

---

## 🧠 Interview Tips

* Samba config file: `/etc/samba/smb.conf`
* Uses **SMB/CIFS**
* Default port: **445**
* `smbpasswd` manages SMB passwords
* `testparm` validates config

---

## 📚 Useful Commands

```bash
smbclient -L localhost
testparm
systemctl restart smbd
```

---

## ✅ Conclusion

Samba is the preferred solution for **cross-platform file sharing**, especially in environments where Linux and Windows systems must coexist.

Proper configuration and permissions ensure secure and reliable file sharing.


---


