
---

## Detailed Steps for Adding New Space with LVM

To add and use new storage, follow this structured 7-step process:

### 1. Install a New Hard Disk Drive

Physically attach the drive to your system or add a virtual disk in your cloud/VM environment. Use `lsblk` to identify the new device (e.g., `/dev/sdb`).

### 2. Make a Partition

While you can use raw disks, it is standard practice to create a partition for LVM.

* **Command:** `fdisk /dev/sdb`
* **Action:** Press `n` (new partition), `p` (primary), and then `t` to change the partition type to **8e** (Linux LVM).

### 3. Designate Physical Volume (PV)

Initialize the partition so LVM can recognize and manage it.

* **Command:** `pvcreate /dev/sdb1`
* **Verification:** `pvs` or `pvdisplay`

### 4. Manage Volume Group (VG)

Create a new Volume Group or extend an existing one to include the new PV.

* **Create New VG:** `vgcreate vg_data /dev/sdb1`
* **Extend Existing VG:** `vgextend vg_system /dev/sdb1`

### 5. Manage Logical Volume (LV)

Carve out a "virtual partition" from your Volume Group.

* **Command:** `lvcreate -L 20G -n lv_storage vg_data`
* *-L 20G:* Specifies a size of 20GB.
* *-n:* Names the logical volume.


* **Verification:** `lvs` or `lvdisplay`

### 6. Apply a Filesystem

Format the Logical Volume so it can store data.

* **For ext4:** `mkfs.ext4 /dev/vg_data/lv_storage`
* **For XFS:** `mkfs.xfs /dev/vg_data/lv_storage`

### 7. Set a Mount Point

Attach the formatted volume to your Linux directory tree.

* **Command:** 1.  `mkdir -p /mnt/my_new_space`
2.  `mount /dev/vg_data/lv_storage /mnt/my_new_space`
* **To make it permanent:** Add an entry to `/etc/fstab`.

---

## Complete Real-World Example

Suppose you just added a **50GB** disk at `/dev/sdc`. Here is the full command sequence:

```bash
# 1. Initialize PV
pvcreate /dev/sdc

# 2. Create Volume Group named 'marketing_vg'
vgcreate marketing_vg /dev/sdc

# 3. Create Logical Volume named 'reports' using 100% of the space
lvcreate -l 100%FREE -n reports marketing_vg

# 4. Format with ext4
mkfs.ext4 /dev/marketing_vg/reports

# 5. Create directory and mount
mkdir -p /data/marketing
mount /dev/marketing_vg/reports /data/marketing

```

