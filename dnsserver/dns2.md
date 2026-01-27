
---

## 📌 Overview

This guide explains **how DNS works end-to-end** and how to **map a custom domain to an Apache web server**.  
You’ll learn:

- DNS fundamentals (A, CNAME, MX, TXT records)
- How a browser resolves a domain
- How to configure DNS for a custom domain
- Apache virtual host setup
- End-to-end example with troubleshooting

---

## 🧠 What is DNS?

**DNS (Domain Name System)** translates human-readable domain names into IP addresses.

Example:
```text
example.com  →  54.182.11.22
````

Without DNS, users would need to remember IP addresses instead of domain names.

---

## 🌍 How DNS Resolution Works (Flow)

```text
User Browser
   ↓
Local DNS Resolver
   ↓
Root DNS Server
   ↓
TLD Server (.com)
   ↓
Authoritative DNS Server
   ↓
IP Address Returned
```

---

## 🧾 Common DNS Record Types

| Record | Purpose                    |
| ------ | -------------------------- |
| A      | Maps domain → IPv4 address |
| AAAA   | Maps domain → IPv6 address |
| CNAME  | Alias to another domain    |
| MX     | Mail server routing        |
| TXT    | Verification, SPF, DKIM    |
| NS     | Name servers for domain    |

---

## 🧪 Example DNS Records

```dns
example.com.        A       54.182.11.22
www.example.com.    CNAME   example.com
example.com.        MX 10   mail.example.com
example.com.        TXT     "v=spf1 include:_spf.google.com ~all"
```

---

## 🖥️ Server Prerequisites

* Linux server (EC2 / VM / Bare metal)
* Public IP attached
* Apache installed
* Domain purchased (GoDaddy, Route53, Namecheap, etc.)

---

## 🔧 Install Apache

### Amazon Linux / RHEL

```bash
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

## 🌐 DNS Configuration (Custom Domain)

### Step 1: Point Domain to Server IP

Create an **A record** in your DNS provider:

| Type | Host        | Value        |
| ---- | ----------- | ------------ |
| A    | example.com | 54.182.11.22 |
| A    | www         | 54.182.11.22 |

⏱ DNS propagation: 5 minutes – 24 hours

---

## 🧪 Verify DNS

```bash
dig example.com
nslookup example.com
ping example.com
```

Expected result:

```text
example.com → 54.182.11.22
```

---

## 📂 Apache Virtual Host Configuration

### Create Web Root

```bash
sudo mkdir -p /var/www/example.com/public_html
sudo chown -R apache:apache /var/www/example.com
```

---

### Create Virtual Host File

```bash
sudo vi /etc/httpd/conf.d/example.com.conf
```

```apache
<VirtualHost *:80>
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html

    <Directory /var/www/example.com/public_html>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog logs/example.com-error.log
    CustomLog logs/example.com-access.log combined
</VirtualHost>
```

---

### Restart Apache

```bash
sudo systemctl restart httpd
```

---

## 🧪 Test Website

```bash
curl http://example.com
```

Or open in browser:

```text
http://example.com
```

---

## 🔐 Enable Firewall Rules

```bash
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```

AWS EC2:

* Allow **Inbound HTTP (80)**
* Allow **Inbound HTTPS (443)**

---

## 🔒 Optional: Enable HTTPS (SSL)

Using **Let’s Encrypt**:

```bash
sudo yum install certbot python3-certbot-apache -y
sudo certbot --apache
```

---

## ❗ Common Issues & Fixes

### ❌ Domain not opening

* DNS not propagated
* Wrong IP in A record
* Security group blocking port 80

### ❌ Apache showing default page

* VirtualHost misconfigured
* `ServerName` mismatch
* Wrong `DocumentRoot`

---

## 📌 End-to-End Example

```text
Domain: example.com
Public IP: 54.182.11.22
DNS: A record → 54.182.11.22
Apache: VirtualHost on port 80
Result: example.com loads custom website
```

---

## 📚 Summary

✔ DNS maps domain to IP
✔ Apache serves content
✔ VirtualHost binds domain to directory
✔ Security + firewall must allow traffic

---

## 🚀 Next Enhancements

* HTTPS auto-renew
* Reverse proxy (Nginx + Apache)
* Load balancer + DNS routing
* CDN integration (CloudFront / Cloudflare)

---
