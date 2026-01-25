
---

# Mastering LVM in Linux

Logical Volume Management (LVM) is a powerful tool for managing disk space in Linux. Unlike traditional partitioning, LVM provides a layer of abstraction between physical storage and the operating system, allowing for much more flexible storage management.

---

## 1. What is LVM and its Example?

**LVM (Logical Volume Manager)** is a device mapper framework that provides logical volume management for the Linux kernel. It allows you to group multiple physical disks into a single storage pool.

### The Architecture

* **Physical Volumes (PV):** Your raw disks or partitions (e.g., `/dev/sdb`, `/dev/sdc`).
* **Volume Groups (VG):** A pool of storage created by combining one or more PVs.
* **Logical Volumes (LV):** The "virtual partitions" carved out of the VG that you format and mount.

**Example:** Imagine you have two 500GB hard drives. With LVM, you can combine them into one 1TB "Volume Group" and then create an 800GB "Logical Volume" for your media files, even though neither physical disk is large enough to hold it alone.

---

## 2. Advantages of LVM

* **Dynamic Resizing:** Shrink or expand partitions without the need for complex re-partitioning.
* **Storage Pooling:** Use space from multiple physical disks as if it were one.
* **Snapshots:** Create a "point-in-time" copy of your data for safe backups.
* **No Reboot Required:** Most LVM operations (like extending a volume) can be done while the system is running.

---

## 3. Possibilities of LVM

With LVM, your storage is no longer static:

* **Live Migration:** Move data across disks while the system is online.
* **Striping:** Improve performance by spreading data across multiple disks (RAID 0 style).
* **Mirroring:** Protect against hardware failure (RAID 1 style).
* **Thin Provisioning:** Allocate more virtual storage than physical space available.

---

## 4. Steps for Adding New Space (Workflow)

Following the standard 7-step process for storage expansion:

### 1. Install a new Hard Disk drive

Physically connect the disk or add a virtual drive. Identify it using `lsblk`.

### 2. Make a partition to use it

* **Command:** `fdisk /dev/sdb`
* **Action:** Create a new partition and set the type to `8e` (Linux LVM).

### 3. Designate Physical Volume (PV)

* **Command:** `pvcreate /dev/sdb1`

### 4. Manage Volume Group (VG)

* **Create New VG:** `vgcreate my_vg /dev/sdb1`
* **Extend Existing VG:** `vgextend existing_vg /dev/sdb1`

### 5. Manage Logical Volume (LV)

* **Command:** `lvcreate -L 10G -n my_lv my_vg`

### 6. Apply a filesystem

* **Command:** `mkfs.ext4 /dev/my_vg/my_lv`

### 7. Set a mount point

* **Command:** `mount /dev/my_vg/my_lv /mnt/storage`

---

## 5. Making it Permanent (/etc/fstab)

To ensure the volume mounts automatically on reboot:

1. Get the UUID: `blkid /dev/my_vg/my_lv`
2. Add this line to `/etc/fstab`:
```text
UUID=xxxx-xxxx-xxxx-xxxx  /mnt/storage  ext4  defaults  0  2

```



---

## 6. LVM Cheat Sheet

| Layer | Create | Display | Scan/List |
| --- | --- | --- | --- |
| **Physical (PV)** | `pvcreate` | `pvdisplay` | `pvs` |
| **Group (VG)** | `vgcreate` | `vgdisplay` | `vgs` |
| **Logical (LV)** | `lvcreate` | `lvdisplay` | `lvs` |

---

## 7. Troubleshooting & Best Practices

* **Activation:** Use `vgchange -ay` to activate VGs if they aren't visible.
* **Shrinking:** Always unmount the volume and run `e2fsck -f` before using `lvreduce`.
* **Naming:** Use prefixes like `vg_` and `lv_` for clarity.
* **Capacity:** Always leave 10-20% of your Volume Group unallocated for snapshots or emergency growth.

---
