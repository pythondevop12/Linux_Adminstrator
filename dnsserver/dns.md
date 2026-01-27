
---

## 1. Environment Overview

| Role | Component | Example Value |
| --- | --- | --- |
| **Server OS** | Ubuntu / Debian | 192.168.1.10 |
| **DNS Software** | BIND9 | port 53 |
| **Web Server** | Apache2 | port 80 |
| **Custom Domain** | Domain Name | `mysite.com` |

---

## 2. Install Necessary Packages

First, update your local package index and install BIND9 and Apache.

```bash
sudo apt update
sudo apt install bind9 bind9utils bind9-doc apache2 -y

```

---

## 3. DNS Configuration (BIND9)

### A. Define the Zone

Edit the local zone file to tell BIND about your custom domain.

**File:** `/etc/bind/named.conf.local`

```text
zone "mysite.com" {
    type master;
    file "/etc/bind/zones/db.mysite.com";
};

```

### B. Create the Zone File

Create the directory and the forward lookup zone file.

```bash
sudo mkdir /etc/bind/zones
sudo cp /etc/bind/db.local /etc/bind/zones/db.mysite.com

```

Edit `/etc/bind/zones/db.mysite.com` and replace the content with your details:

```text
;
; BIND data file for mysite.com
;
$TTL    604800
@       IN      SOA     ns1.mysite.com. admin.mysite.com. (
                              3         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ns1.mysite.com.
@       IN      A       192.168.1.10
ns1     IN      A       192.168.1.10
www     IN      A       192.168.1.10

```

### C. Check Configuration and Restart

```bash
sudo named-checkconf
sudo named-checkzone mysite.com /etc/bind/zones/db.mysite.com
sudo systemctl restart bind9

```

---

## 4. Apache Web Server Configuration

### A. Create Directory Structure

```bash
sudo mkdir -p /var/www/mysite.com/public_html
sudo chown -R $USER:$USER /var/www/mysite.com/public_html

```

### B. Create a Sample Page

Create an `index.html` file in your new directory.

```html
<h1>Success! mysite.com is working with BIND9.</h1>

```

### C. Configure Virtual Host

Create a new configuration file for your domain.

**File:** `/etc/apache2/sites-available/mysite.com.conf`

```apache
<VirtualHost *:80>
    ServerAdmin admin@mysite.com
    ServerName mysite.com
    ServerAlias www.mysite.com
    DocumentRoot /var/www/mysite.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

```

### D. Enable Site and Restart

```bash
sudo a2ensite mysite.com.conf
sudo a2dissite 000-default.conf
sudo systemctl restart apache2

```

---

## 5. Testing the Setup

To verify that your DNS is resolving correctly, run the following from a client machine on the same network:

1. **Set DNS Server:** Change your client's DNS settings to point to `192.168.1.10`.
2. **Ping the domain:**
```bash
ping mysite.com

```


3. **Web Access:** Open a browser and navigate to `http://mysite.com`.

---

## Troubleshooting

* **Firewall:** Ensure ports 53 (TCP/UDP) and 80 (TCP) are open.
`sudo ufw allow 53`
`sudo ufw allow 80`
* **Syntax Errors:** Use `apache2ctl configtest` for Apache and `named-checkconf` for BIND.

---