# Day 13 – Linux Volume Management (LVM)

# Objective

Today I learned Linux Volume Management (LVM) and practiced:
- Creating Physical Volumes (PV)
- Creating Volume Groups (VG)
- Creating Logical Volumes (LV)
- Formatting and mounting volumes
- Extending storage dynamically

LVM provides flexible storage management in Linux systems.

---

# Step 1 – Switch to Root User

## Input
```bash
sudo -i
```

## Output
```bash
root@ubuntu:~#
```

### Observation
- Switched successfully to root user for administrative tasks.

---

# Step 2 – Create Virtual Disk (Loop Device)

## Create Disk Image

### Input
```bash
dd if=/dev/zero of=/tmp/disk1.img bs=1M count=1024
```

### Output
```bash
1024+0 records in
1024+0 records out
1073741824 bytes copied
```

---

## Attach Loop Device

### Input
```bash
losetup -fP /tmp/disk1.img
```

---

## Verify Loop Device

### Input
```bash
losetup -a
```

### Output
```bash
/dev/loop0: []: (/tmp/disk1.img)
```

### Observation
- Virtual disk successfully attached as `/dev/loop0`.

---

# Task 1 – Check Current Storage

## Check Block Devices

### Input
```bash
lsblk
```

### Output
```bash
sda      50G
loop0     1G
```

---

## Check Physical Volumes

### Input
```bash
pvs
```

### Output
```bash
No physical volume found
```

---

## Check Volume Groups

### Input
```bash
vgs
```

### Output
```bash
No volume groups found
```

---

## Check Logical Volumes

### Input
```bash
lvs
```

### Output
```bash
No logical volumes found
```

---

## Check Disk Usage

### Input
```bash
df -h
```

### Output
```bash
Filesystem      Size  Used Avail Use%
/dev/sda1        50G   18G   30G  38%
```

### Observation
- System currently has no LVM configuration.

---

# Task 2 – Create Physical Volume

## Create Physical Volume

### Input
```bash
pvcreate /dev/loop0
```

### Output
```bash
Physical volume "/dev/loop0" successfully created.
```

---

## Verify PV

### Input
```bash
pvs
```

### Output
```bash
PV           VG   Fmt  Attr PSize PFree
/dev/loop0        lvm2 ---  1.00g 1.00g
```

### Observation
- Physical volume created successfully.

---

# Task 3 – Create Volume Group

## Create VG

### Input
```bash
vgcreate devops-vg /dev/loop0
```

### Output
```bash
Volume group "devops-vg" successfully created
```

---

## Verify VG

### Input
```bash
vgs
```

### Output
```bash
VG         #PV #LV #SN Attr   VSize VFree
devops-vg    1   0   0 wz--n- 1.00g 1.00g
```

### Observation
- Volume group successfully created.

---

# Task 4 – Create Logical Volume

## Create LV

### Input
```bash
lvcreate -L 500M -n app-data devops-vg
```

### Output
```bash
Logical volume "app-data" created.
```

---

## Verify LV

### Input
```bash
lvs
```

### Output
```bash
LV        VG         Attr       LSize
app-data  devops-vg -wi-a----- 500.00m
```

### Observation
- Logical volume created successfully.

---

# Task 5 – Format and Mount

## Format Logical Volume

### Input
```bash
mkfs.ext4 /dev/devops-vg/app-data
```

### Output
```bash
Filesystem created successfully
```

---

## Create Mount Point

### Input
```bash
mkdir -p /mnt/app-data
```

---

## Mount Volume

### Input
```bash
mount /dev/devops-vg/app-data /mnt/app-data
```

---

## Verify Mount

### Input
```bash
df -h /mnt/app-data
```

### Output
```bash
Filesystem                      Size  Used Avail Use%
/dev/mapper/devops--vg-app--data 477M  24M  420M   6%
```

### Observation
- Logical volume mounted successfully.

---

# Task 6 – Extend the Volume

## Extend Logical Volume

### Input
```bash
lvextend -L +200M /dev/devops-vg/app-data
```

### Output
```bash
Size of logical volume changed from 500 MiB to 700 MiB
```

---

## Resize Filesystem

### Input
```bash
resize2fs /dev/devops-vg/app-data
```

### Output
```bash
Filesystem resized successfully
```

---

## Verify Extended Size

### Input
```bash
df -h /mnt/app-data
```

### Output
```bash
Filesystem                      Size  Used Avail Use%
/dev/mapper/devops--vg-app--data 677M  25M  620M   5%
```

### Observation
- Storage extended successfully without recreating the filesystem.

---

# Commands Used

```bash
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

# What I Learned

- LVM allows flexible storage management in Linux.
- Physical Volumes combine into Volume Groups.
- Logical Volumes can be resized dynamically.
- Filesystems can be expanded without reinstalling Linux.
- LVM is widely used in servers and cloud environments.

---

# Final Summary

Today I practiced:
- Creating and managing LVM storage
- Mounting filesystems
- Dynamically extending storage volumes

This challenge improved my understanding of Linux storage management and real-world server administration concepts.

#90DaysOfDevOps
#DevOpsKaJosh
#TrainWithShubham
