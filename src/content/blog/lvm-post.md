---
author: nihalebr
pubDatetime: 2023-05-29T19:30:35+05:30
title: Logical Volume Manager (LVM) 💽
postSlug: lvm
featured: true
ogImage: https://raw.githubusercontent.com/nihalebr/LVM/main/lvm-diagram3.png
tags:
  - lvm
  - RHEL
  - Linux os
description: LVM is a logical volume manager for the Linux kernel that allows you to manage disk storage. It is a device mapper target that provides logical volume management for the Linux kernel. 
---

LVM is a logical volume manager for the Linux kernel that allows you to manage disk storage. It is a device mapper target that provides logical volume management for the Linux kernel. 

LVM combines multiple physical disk drives into a single “virtual” drive (Volume group). This virtual drive can then be divided into one or more logical volumes. Each logical volume can be formatted with a filesystem and mounted as a separate drive.

![LVM](https://raw.githubusercontent.com/nihalebr/LVM/main/lvm-diagram3.png)

## Advantages of LVM

* LVM allows you to easily add or remove storage space to a system without having to reconfigure the filesystem.

* Keeping the filesystem separate from the physical storage allows you to easily move the filesystem to a different physical storage device.

* LVM allows you to easily resize the filesystem without having to reconfigure the filesystem.

## Disadvantages of LVM

* LVM is not a filesystem. It is a logical volume manager. It does not provide any filesystem features.

* LVM does not provide any data protection features. You need to use a filesystem that provides data protection features.

## LVM Components

* Physical Volume (PV) : A physical volume is a partition or a whole disk that is used by LVM.

* Volume Group (VG) : A volume group is a collection of one or more physical volumes. A volume group is used to create a logical volume.

* Logical Volume (LV) : A logical volume is a partition that is created from a volume group. A logical volume can be formatted with a filesystem and mounted as a separate drive.

## How data is stored in LVM

![LVM](https://raw.githubusercontent.com/nihalebr/LVM/main/lvm-diagram2.png)

## Usage

Before creation of a Physical Volume, you need to create a partition on the disk. You can use fdisk or gdisk to create a partition.
(or you can use a whole disk as a Physical Volume).

![lvm-managment](https://raw.githubusercontent.com/nihalebr/LVM/main/lvm-diagram.png)

### Create a Physical Volume
```bash
pvcreate /dev/sda2 /dev/sdb2
```
After creating a Physical Volume, you can create a Volume Group.

### Create a Volume Group
```bash
vgcreate vg0 /dev/sda2 /dev/sdb2
```

Finally, you can create a Logical Volume.To create a Logical Volume, you need to specify the size of the Logical Volume. You can specify the size in megabytes (MB), gigabytes (GB), or terabytes (TB). Also, you can specify the size as No. of Physical Extents (PEs).

### Create a Logical Volume using size in MB or GB
```bash
lvcreate -L 1024MB -n lv0 vg0
```
### Create a Logical Volume using No. of Physical Extents (PEs)
```bash
lvcreate -l 50 -n lv0 vg0
```

After creating a Logical Volume, you can format it with a filesystem and mount it.
 
---

## Support for lvextend and lvreduce

Some filesystem support resizing.while some filesystem does not supports resizing. 

The following table shows the support for resizing in some file system.

![lvm-resize](https://raw.githubusercontent.com/nihalebr/LVM/main/lvm-table.png)

### To expand a Logical Volume ( lvextend )
```bash
lvextend -L +1024MB /dev/vg0/lv0
```
### To shrink a Logical Volume( lvreduce )
```bash
lvreduce -L -1024MB /dev/vg0/lv0
```

---

[🔝 Back to Usage](#usage)
