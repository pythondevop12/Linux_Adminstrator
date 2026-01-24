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



# Linux User Management Guide

A cheat sheet for creating, modifying, and deleting users and groups in Linux.

## Table of Contents
- [1. List Users](#1-list-users)
- [2. Create Users](#2-create-users)
- [3. Delete Users](#3-delete-users)
- [4. Modify Users](#4-modify-users)
- [5. Group Management](#5-group-management)
- [6. Password Management](#6-password-management)

---

Managing users and groups is a fundamental skill for any Linux administrator. In Linux, everything is a file, and permissions are tied to these user and group identities.

Here is your detailed guide to Linux User and Group Management.

---

## 1. List Users

To manage users, you first need to know who is on the system. Linux stores user information in local text files.

* **View all users:** The `/etc/passwd` file contains a list of all user accounts.
```bash
cat /etc/passwd

```


* **Show logged-in users:** To see who is currently active:
```bash
who
# or
w

```


* **Check a specific user:** Use the `id` command to see UID (User ID) and GID (Group ID).
```bash
id username

```



---

## 2. Create Users

Adding a new user involves creating their home directory and setting up their environment.

* **The standard command:** `useradd` is the low-level utility, while `adduser` is often a more interactive, user-friendly script on Debian-based systems.
```bash
sudo useradd -m newuser  # -m creates the home directory

```


* **Create user with specific shell:**
```bash
sudo useradd -s /bin/bash newuser

```



---

## 3. Delete Users

Removing a user requires caution, especially regarding their saved files.

* **Remove user only:**
```bash
sudo userdel username

```


* **Remove user and their home directory:** This is usually the preferred method to keep the system clean.
```bash
sudo userdel -r username

```



---

## 4. Modify Users

The `usermod` command allows you to change existing account attributes.

* **Change username:**
```bash
sudo usermod -l newname oldname

```


* **Lock/Unlock an account:**
```bash
sudo usermod -L username  # Lock
sudo usermod -U username  # Unlock

```


* **Change home directory:**
```bash
sudo usermod -d /home/new_home -m username

```



---

## 5. Group Management

Groups allow you to apply permissions to multiple users at once.

* **Create a group:**
```bash
sudo groupadd developers

```


* **Add user to a group:** Use the `-aG` (append to supplementary groups) flags.
```bash
sudo usermod -aG developers username

```


* **Remove a group:**
```bash
sudo groupdel developers

```



---

## 6. Password Management

Security starts with robust password handling.

* **Change a user's password:**
```bash
sudo passwd username

```


* **Force a user to change password at next login:**
```bash
sudo chage -d 0 username

```


* **Set password expiration:**
```bash
sudo chage -M 90 username  # Expires in 90 days

```



---
