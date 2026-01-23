# Linux Getting Started Guide

This roadmap outlines the foundational steps for setting up and understanding a Linux environment.

## 1. Install a Linux Distribution
Before you can learn, you need a working environment. You can install Linux directly on hardware or use virtualization software to run it safely alongside your current operating system.

* **Choose a Distribution (Distro):**
    * *Beginner-friendly:* Ubuntu, Linux Mint, Pop!_OS.
    * *Intermediate:* Fedora, Debian, Manjaro.
* **Installation Methods:**
    * **Virtual Machine (Recommended for beginners):** Use tools like VirtualBox or VMware Workstation Player to run Linux as a program within Windows or macOS.
    * **WSL (Windows Subsystem for Linux):** Run a Linux terminal directly inside Windows 10/11 without a dual boot.
    * **Dual Boot:** Partition your hard drive to choose between Windows and Linux at startup.

## 2. Learn Basic Commands
The terminal is the most powerful tool in Linux. Master these core commands to navigate and manipulate files efficiently.

* **Navigation & File Management:**
    * `ls`: List directory contents (see what files are in the folder).
    * `cd`: Change directory (move to a different folder).
    * `pwd`: Print working directory (shows where you currently are).
    * `cp`: Copy files or directories.
    * `mv`: Move or rename files.
    * `rm`: Remove (delete) files. *Use with caution!*
    * `mkdir`: Make a new directory (folder).
    * `touch`: Create an empty file.
* **Getting Help:**
    * `man <command>`: Displays the manual page for a command (e.g., `man ls` tells you how to use the list command).

## 3. Familiarize with the File System
Linux uses a hierarchical file system structure that is different from Windows. Understanding where files live is crucial for administration.

* **The Root Directory (`/`):** The top-level directory containing all other files.
* **Common Directories:**
    * `/home`: Contains personal user files (similar to `C:\Users\` in Windows). Each user has their own subdirectory here.
    * `/etc`: The configuration center. Contains system-wide configuration files and startup scripts.
    * `/var`: Variable data files. Used for logs (`/var/log`), spool files, and temporary cache files.
    * `/usr`: User System Resources. Contains the majority of user utilities and applications.
    * `/bin` & `/sbin`: Essential command binaries (programs) like `ls` and `cp`.
    * `/tmp`: Temporary files that are usually deleted upon reboot.