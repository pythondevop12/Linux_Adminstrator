# Linux System Administration Roadmap & Guide

This comprehensive guide covers the path from beginner to advanced Linux administration, including a detailed command playlist for user management.

---

## 1. Getting Started
Before diving into administration, you must establish a working environment and understand the basics.

### Install a Linux Distribution
Set up a Linux system on your computer or use a virtual machine.
* **Beginner Distros:** Ubuntu, Linux Mint, Pop!_OS.
* **Methods:** VirtualBox/VMware, WSL (Windows Subsystem for Linux), or Dual Boot.

### Learn Basic Commands
Understand and practice basic Linux commands to navigate the CLI.
* **Navigation:** `ls` (list), `cd` (change dir), `pwd` (print working dir).
* **File Ops:** `cp` (copy), `mv` (move/rename), `rm` (remove), `mkdir` (make dir), `touch` (create file).
* **Help:** `man <command>` (manual pages).

### Familiarize with the File System
Learn the Linux file system hierarchy.
* `/`: Root directory.
* `/home`: User personal files.
* `/etc`: Configuration files.
* `/var`: Variable data (logs, spool).
* `/usr`: User programs and utilities.

---

## 2. Core Administration
These skills are the daily bread and butter of a sysadmin: managing who can access the system and what they can do.

### User and Group Management (Overview)
Learn to add, modify, and delete users and groups.
* *See the "Deep Dive" section at the end of this file for a detailed command playlist.*

### File Permissions and Ownership
Understand how to manage file security.
* **Chmod:** Change read (r), write (w), and execute (x) permissions.
* **Chown:** Change file ownership (user).
* **Chgrp:** Change group ownership.

### Process Management
Get familiar with managing running applications.
* **Monitoring:** `ps`, `top`, `htop`.
* **Control:** `kill` (stop processes), `nice` (adjust priority).

---

## 3. Networking & Security
Connecting your server to the world securely is critical.

### Networking Basics
Configure interfaces and troubleshoot connectivity.
* **Config:** `ip` (modern), `ifconfig` (legacy).
* **Troubleshooting:** `ping` (connectivity), `traceroute` (path analysis).
* **Tasks:** Setting static IP addresses.

### Firewall Configuration
Manage incoming and outgoing network traffic.
* **Tools:** `ufw` (simple), `firewalld` (zones), `iptables` (core).

### SSH Configuration
Secure remote access to your server.
* **Best Practices:** Configure key-based authentication (passwordless), disable root login, and manage SSH access.

---

## 4. Observability & Maintenance
Keep the system healthy, audit actions, and prevent data loss.

### System Monitoring
Monitor system performance in real-time.
* **Tools:** `top`, `htop`, `vmstat` (virtual memory stats), `iostat` (input/output stats).

### Log Management
Learn to read and manage system logs.
* **Location:** `/var/log`.
* **Tools:** `journalctl` (systemd logs), `logrotate` (managing log file sizes).

### Backup and Restore
Implement strategies to save data.
* **Tools:** `rsync` (sync files), `tar` (archive/zip), `cron` (schedule automated backups).

---

## 5. Storage Management
Handling disks, partitions, and file systems.

### Storage and Filesystem

* **Partitioning:** `fdisk`, `parted`.
* **File System Mgmt:** `mkfs` (format), `fsck` (check/repair), `resize2fs`, `tune2fs`.
* **Mounting:** `mount`, `umount`, and configuring `/etc/fstab` for permanent mounts.

---

## 6. Professional Growth


### Certifications
Consider validating your skills with:
* CompTIA Linux+
* Red Hat Certified System Administrator (RHCSA)
* Linux Professional Institute Certification (LPIC)

### Stay Updated & Practice
* Follow Linux blogs and forums.
* Continuously practice on real-world projects to gain hands-on experience.

---

## Appendix: Deep Dive - User Management Playlist
A specific command reference for managing users and groups.

### 1. Creating Users (`useradd`)
* **Command:** `sudo useradd [options] username`
* **Common Flags:**
    * `-m`: Create home directory.
    * `-s /bin/bash`: Set default shell.
    * `-G groupname`: Add to secondary group.
* **Example:** `sudo useradd -m -s /bin/bash -G sudo alex`

### 2. Managing Passwords (`passwd`)
* **Set Password:** `sudo passwd username`
* **Force Change:** `sudo passwd -e username` (User must change pass on next login).
* **Lock/Unlock:** `sudo passwd -l username` / `sudo passwd -u username`.

### 3. Modifying Users (`usermod`)
* **Add to Group:** `sudo usermod -aG groupname username` (Always use `-a` to append!).
* **Change Shell:** `sudo usermod -s /bin/zsh username`.
* **Lock Account:** `sudo usermod -L username`.

### 4. Deleting Users (`userdel`)
* **Remove User Only:** `sudo userdel username`.
* **Remove User & Home Dir:** `sudo userdel -r username` (Be careful!).

### 5. Group Management
* **Create Group:** `sudo groupadd groupname`.
* **Delete Group:** `sudo groupdel groupname`.
* **Check Membership:** `id username` or `cat /etc/group`.