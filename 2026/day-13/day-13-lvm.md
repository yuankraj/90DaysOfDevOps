# Day 13 – Linux Volume Management (LVM)

## Objective
Learn how to manage storage using **Linux Logical Volume Manager (LVM)**.  
This includes creating physical volumes, volume groups, logical volumes, formatting them, mounting them, and extending storage.

---

# Environment Setup

Switched to root user:

```bash
sudo -i
```

---

# Create Virtual Disk (If No Extra Disk Available)

Create a virtual disk file:

```bash
dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024
```

Example Output

```
1024+0 records in
1024+0 records out
1073741824 bytes copied
```

Attach disk to loop device:

```bash
losetup -fP /tmp/disk1.img
```

Check loop devices:

```bash
losetup -a
```

Example Output

```
/dev/loop0: [2065]: (/tmp/disk1.img)
```

---

# Task 1 – Check Current Storage

Check block devices:

```bash
lsblk
```

Check physical volumes:

```bash
pvs
```

Check volume groups:

```bash
vgs
```

Check logical volumes:

```bash
lvs
```

Check disk usage:

```bash
df -h
```

Observation  
Initially there are **no LVM volumes created**.

---

# Task 2 – Create Physical Volume

Create physical volume:

```bash
pvcreate /dev/loop0
```

Verify:

```bash
pvs
```

Example Output

```
PV         VG  Fmt  Attr PSize  PFree
/dev/loop0     lvm2 ---  1.00g  1.00g
```

Observation  
The loop device is now recognized as a **physical volume**.

---

# Task 3 – Create Volume Group

Create volume group:

```bash
vgcreate devops-vg /dev/loop0
```

Verify:

```bash
vgs
```

Example Output

```
VG        #PV #LV #SN Attr   VSize  VFree
devops-vg   1   0   0 wz--n- 1.00g  1.00g
```

Observation  
Volume group **devops-vg** is created successfully.

---

# Task 4 – Create Logical Volume

Create logical volume:

```bash
lvcreate -L 500M -n app-data devops-vg
```

Verify:

```bash
lvs
```

Example Output

```
LV        VG        Attr       LSize
app-data  devops-vg -wi-a----- 500.00m
```

Observation  
Logical volume **app-data** created with size **500MB**.

---

# Task 5 – Format and Mount

Format logical volume:

```bash
mkfs.ext4 /dev/devops-vg/app-data
```

Create mount directory:

```bash
mkdir -p /mnt/app-data
```

Mount logical volume:

```bash
mount /dev/devops-vg/app-data /mnt/app-data
```

Verify mount:

```bash
df -h /mnt/app-data
```

Example Output

```
Filesystem                     Size Used Avail Use%
/dev/mapper/devops--vg-app--data 488M  24K  450M   1% /mnt/app-data
```

Observation  
Logical volume successfully mounted.

---

# Task 6 – Extend the Volume

Extend logical volume by 200MB:

```bash
lvextend -L +200M /dev/devops-vg/app-data
```

Resize filesystem:

```bash
resize2fs /dev/devops-vg/app-data
```

Verify new size:

```bash
df -h /mnt/app-data
```

Example Output

```
Filesystem                     Size Used Avail Use%
/dev/mapper/devops--vg-app--data 680M  24K  650M   1% /mnt/app-data
```

Observation  
Logical volume successfully extended.

---

# Commands Used

```
sudo -i
dd
losetup
lsblk
pvs
vgs
lvs
pvcreate
vgcreate
lvcreate
mkfs.ext4
mount
df -h
lvextend
resize2fs
```

---

# Screenshots

Add screenshots for the following steps:

```
lsblk-output.png
pvcreate-output.png
vgcreate-output.png
lvcreate-output.png
mount-output.png
volume-extend.png
```

---

# What I Learned

1. LVM allows flexible disk management by creating logical volumes on top of physical storage.
2. Logical volumes can be **extended without downtime**, which is useful for production servers.
3. LVM provides better storage management compared to traditional partitioning.

---

# Summary

In this exercise, I practiced Linux Logical Volume Management by creating a physical volume, grouping it into a volume group, creating a logical volume, formatting and mounting it, and finally extending its storage. LVM is widely used in DevOps environments to manage storage dynamically and efficiently.
