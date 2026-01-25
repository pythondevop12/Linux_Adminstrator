````md
# Linux Package Management Systems: rpm, dpkg, apt, yum and More


# Package Management

## It's all about packages
* **Installing**
* **Upgrading**
* **Deleting**
* **View Package Info**

---

## Package Management Overview
* **What is package management?**
    * The process of installing, updating, configuring, and removing software packages in a consistent manner.
* **Package management tools**
    * `yum` (Yellowdog Updater, Modified - RHEL/CentOS 7 and older)
    * `dnf` (Dandified YUM - Modern RHEL/CentOS/Fedora)
    * `rpm` (Red Hat Package Manager - Low-level tool)
    * `apt` (Advanced Package Tool - Debian/Ubuntu)
* **Difference between Upgrade or Update?**
    * **Update:** Usually refreshes the package list (repository metadata) to see what's available.
    * **Upgrade:** Actually downloads and installs the newer versions of the software.
* **Rollback the patches or updates**
    * Reverting a package to a previous version if an update causes issues.


## Overview
Linux distributions use different package management systems to install, upgrade, remove, and manage software.
Each system has its own tools, package formats, and repositories.

This document explains **rpm, dpkg, apt, yum**, and related tools in detail for use in a `README.md`.

---

## High-Level Package Management Architecture

Linux package management generally consists of:
- **Package format** (e.g., `.rpm`, `.deb`)
- **Low-level package manager** (installs/removes packages)
- **High-level package manager** (resolves dependencies, manages repositories)

---

## RPM (Red Hat Package Manager)

### What is RPM
- Package format: `.rpm`
- Low-level package management system
- Used by Red Hat–based distributions

### Common RPM-Based Distributions
- RHEL
- CentOS
- Rocky Linux
- AlmaLinux
- Fedora
- Amazon Linux

### What RPM Does
- Installs and removes packages
- Verifies package integrity
- Queries installed packages
- Does **not** resolve dependencies automatically

### Common RPM Commands
```bash
rpm -ivh package.rpm      # Install
rpm -Uvh package.rpm      # Upgrade
rpm -e package            # Remove
rpm -qa                   # List installed packages
rpm -qi package           # Package info
rpm -ql package           # List files in package
rpm -qf /path/file        # Find owning package
````

---

## YUM (Yellowdog Updater Modified)

### What is YUM

* High-level package manager for RPM systems
* Handles dependency resolution
* Works with remote repositories

### Used In

* RHEL 7 and below
* CentOS 7
* Amazon Linux 2

### Common YUM Commands

```bash
yum install package
yum remove package
yum update
yum list installed
yum search keyword
yum info package
```

---

## DNF (Dandified YUM)

### What is DNF

* Modern replacement for YUM
* Faster and more reliable dependency resolution
* Default in newer RPM-based systems

### Used In

* RHEL 8+
* Rocky Linux
* AlmaLinux
* Fedora

### Common DNF Commands

```bash
dnf install package
dnf remove package
dnf update
dnf list installed
dnf search keyword
dnf info package
```

---

## DPKG (Debian Package Manager)

### What is DPKG

* Low-level package manager for Debian-based systems
* Package format: `.deb`
* Does **not** resolve dependencies automatically

### Used In

* Debian
* Ubuntu
* Linux Mint

### Common DPKG Commands

```bash
dpkg -i package.deb       # Install
dpkg -r package           # Remove
dpkg -P package           # Purge
dpkg -l                   # List installed packages
dpkg -s package           # Package status
dpkg -L package           # List files
dpkg -S /path/file        # Find owning package
```

---

## APT (Advanced Package Tool)

### What is APT

* High-level package manager for Debian-based systems
* Handles dependencies and repositories
* Works on top of `dpkg`

### Used In

* Ubuntu
* Debian
* Linux Mint

### Common APT Commands

```bash
apt update
apt install package
apt remove package
apt purge package
apt upgrade
apt list --installed
apt search keyword
apt show package
```

---

## APT-GET vs APT

| Feature               | apt | apt-get |
| --------------------- | --- | ------- |
| User-friendly output  | ✅   | ❌       |
| Recommended for users | ✅   | ❌       |
| Script stability      | ❌   | ✅       |
| Interactive usage     | ✅   | ❌       |

---

## Zypper

### What is Zypper

* Package manager for SUSE-based systems
* Uses RPM packages

### Used In

* SUSE Linux Enterprise
* openSUSE

### Common Zypper Commands

```bash
zypper install package
zypper remove package
zypper update
zypper search keyword
zypper info package
```

---

## Snap (Universal Package System)

### What is Snap

* Cross-distribution package system
* Packages are containerized

### Characteristics

* Includes all dependencies
* Auto-updates
* Larger package size

### Common Snap Commands

```bash
snap list
snap install package
snap remove package
snap refresh
```

---

## Flatpak (Universal Package System)

### What is Flatpak

* Universal application distribution system
* Focused on desktop applications

### Common Flatpak Commands

```bash
flatpak list
flatpak install package
flatpak uninstall package
flatpak update
```

---

## Package Manager Comparison

| Distribution | Low-Level | High-Level |
| ------------ | --------- | ---------- |
| RHEL         | rpm       | yum / dnf  |
| CentOS 7     | rpm       | yum        |
| CentOS 8+    | rpm       | dnf        |
| Ubuntu       | dpkg      | apt        |
| Debian       | dpkg      | apt        |
| SUSE         | rpm       | zypper     |

---

## Key Takeaways

* `rpm` and `dpkg` are low-level tools
* `yum`, `dnf`, and `apt` handle dependencies
* Ubuntu uses `dpkg` + `apt`
* RHEL uses `rpm` + `dnf`
* Use high-level tools for daily operations
* Use low-level tools for debugging and audits

---

## Interview-Ready Summary

> Linux package management consists of low-level tools like rpm and dpkg that handle package installation, and high-level tools like apt, yum, and dnf that resolve dependencies and manage repositories. Ubuntu uses dpkg and apt, while RHEL-based systems use rpm with yum or dnf.

```
```


Here is the updated Markdown note. I've added the specific commands for the most common package managers (`dnf/yum` for RHEL-based systems and `apt` for Debian/Ubuntu) and detailed the management lifecycle.

---

# Package Management: Detailed Commands

## 1. Core Package Operations

This covers the basic lifecycle of software on a Linux system.

### Installing Packages

* **APT:** `sudo apt install <package_name>`
* **DNF/YUM:** `sudo dnf install <package_name>`

### Deleting (Removing) Packages

* **APT:** `sudo apt remove <package_name>` (Use `purge` to remove configuration files too).
* **DNF/YUM:** `sudo dnf remove <package_name>`

### Viewing Package Info

* **APT:** `apt show <package_name>`
* **DNF/YUM:** `dnf info <package_name>`
* **List Installed:** `apt list --installed` or `dnf list installed`

---

## 2. Update vs. Upgrade

Understanding this distinction is critical for system stability.

| Action | Command (APT) | Command (DNF) | Purpose |
| --- | --- | --- | --- |
| **Update** | `sudo apt update` | `sudo dnf check-update` | Refreshes the local cache of available packages from the repositories. It does **not** install anything. |
| **Upgrade** | `sudo apt upgrade` | `sudo dnf upgrade` | Compares installed versions to the "Updated" list and installs the newer versions. |

---

## 3. Package Management Tools Overview

* **Low-Level Tools:**
* `rpm`: Manages `.rpm` files directly (Red Hat).
* `dpkg`: Manages `.deb` files directly (Debian).
* *Note: These do not handle dependencies automatically.*


* **High-Level Tools (Wrappers):**
* `yum` / `dnf`: Automated dependency resolution for Red Hat/Fedora.
* `apt`: Automated dependency resolution for Ubuntu/Debian.



---

## 4. Rollbacks and Maintenance

If an update breaks the system, you need to revert or clean up.

### Rollback (DNF Only)

DNF has a powerful "History" feature that APT lacks natively:

* **View History:** `sudo dnf history`
* **Undo a specific action:** `sudo dnf history undo <ID>`
* **Rollback to a state:** `sudo dnf history rollback <ID>`

### Downgrading (APT)

In APT, you must specify the version manually:

* `sudo apt install <package_name>=<version_number>`

### Cleaning up

* **Remove unused dependencies:** `sudo apt autoremove` or `sudo dnf autoremove`
* **Clear cache:** `sudo apt clean` or `sudo dnf clean all`

---
