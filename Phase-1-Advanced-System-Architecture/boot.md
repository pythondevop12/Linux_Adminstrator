# Linux Boot Process: From Power-On to Systemd Targets

---

## 1. BIOS / UEFI Stage
The first phase involves the motherboard firmware initializing hardware and locating the bootable media.

### BIOS (Legacy)
- **POST (Power-On Self-Test):** Checks CPU, RAM, and essential hardware.
- **MBR (Master Boot Record):** BIOS looks for the first 512 bytes of the primary drive to find the Stage 1 bootloader.

### UEFI (Modern)
- **GPT Support:** Handles disks larger than 2TB.
- **EFIVARS:** Stores boot configuration in non-volatile memory.
- **ESP (EFI System Partition):** Loads `.efi` executables directly from a FAT32 partition.



---

## 2. The Bootloader (GRUB2)
GRUB2 (Grand Unified Bootloader) is the most common bootloader for modern Linux distributions.

1. **Stage 1:** Loaded by BIOS/UEFI; its only job is to load Stage 2.
2. **Stage 2:** Loads the filesystem drivers to read the configuration file located at `/boot/grub/grub.cfg`.
3. **Kernel Loading:** GRUB loads the selected **Kernel Image (vmlinuz)** and the **Initramfs** into memory.



---

## 3. Kernel Initialization
The kernel is the heart of the OS. When it starts, it has limited access to hardware.

- **Initramfs (Initial RAM Filesystem):** A temporary root filesystem containing drivers for hardware (like RAID, LVM, or encrypted drives).
- **Driver Loading:** The kernel mounts the real root filesystem (`/`) from the disk.
- **PID 1:** The kernel terminates the initramfs and starts the first user-space process: `/sbin/init` (linked to `systemd`).

---

## 4. Systemd Targets
Once `systemd` (Process ID 1) starts, it initializes the system using **Targets**, which are groups of services that should be running together.

### Comparison of Systemd Targets vs. SysVinit Runlevels

| Systemd Target | Legacy Runlevel | Description |
| :--- | :--- | :--- |
| `poweroff.target` | 0 | System shutdown. |
| `rescue.target` | 1 | Single-user mode (repair console). |
| `multi-user.target` | 3 | Multi-user, non-graphical (Server standard). |
| `graphical.target` | 5 | Multi-user, graphical (Desktop standard). |
| `reboot.target` | 6 | System restart. |



---

## 5. Summary Flowchart
1. **Hardware Power-on** $\rightarrow$ **POST**.
2. **Firmware** $\rightarrow$ BIOS/UEFI locates the Bootloader.
3. **Bootloader** $\rightarrow$ GRUB2 loads Kernel and Initramfs.
4. **Kernel** $\rightarrow$ Mounts real root `/` and starts `systemd`.
5. **Systemd** $\rightarrow$ Reaches the default **Target** (CLI or GUI).

---

## Useful Commands
```bash
# Analyze boot time
systemd-analyze

# See time taken by each service during boot
systemd-analyze blame

# Check logs for the current boot
journalctl -b

# See current default target
systemctl get-default