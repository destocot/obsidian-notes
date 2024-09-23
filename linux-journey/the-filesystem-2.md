---
title: "The Filesystem 2"
sidebar:
  order: 15
---

## Disk Partitioning

There are many tools available to partition our disk:

- **fdisk** - basic command-line partitioning tool, it does not support GPT
- **parted** - command line tool that supports both MBR and GPT partitioning
- **gparted** - GUI version of parted
- **gdisk** - fdisk, but it does not support MBR only GPT

```bash
# launch parted
sudo parted

# select device
select /dev/sdb2

# view current partition table
print

# partition the device
mkpart primary 123 4567

# resize a partition
resize 2 1245 3456
```

## Creating Filesystems

```bash
sudo mkfs -t ext4 /dev/sdb2
```

The `mkfs` (make filesystem) tool allows us to specify the type of filesystem we want and where we want it.

## mount and unmount

Before you can view the contents of your filesystem, you will have to mount it.

To do that you'll need the device location, the filesystem type, and a mount point.

A mount point is a directory on the system where the filesystem is going to be attached.

```bash
# mount the device '/dev/sdv2' of filesystem type ext4 to '/mydrive'
sudo mount -t ext4 /dev/sdb2 /mydrive

# unmount device
sudo unmount /dev/sdb2
# or sudo unmount/mydrive
```

```bash
# view the UUIDs on your system for block devices
sudo blkid

#unmount device with UUID
sudo unmount UUID=b1f2e2d0-d493-43b5-8707-f5a304f05eae /mydrive
```

## /etc/fstab

When we want to automatically mount filesystems at startup we can add them to a file called `/etc/fstab` (pronounced "eff es tab")

```bash
cat /etc/fstab
```

```
UUID=130b882f-7d79-436d-a096-1e594c92bb76 /               ext4    relatime,errors=remount-ro 0       1
UUID=78d203a0-7c18-49bd-9e07-54f44cdb5726 /home           xfs     relatime        0       2
UUID=22c3d34b-467e-467c-b44d-f03803c2c526 none            swap    sw              0       0
```

Each line represents one filesystem

- **UUID** - device identifier
- **Mount point** - Directory the filesystem is mounted to
- **Filesystem type**
- **Options** - other mount options, see manpage for more details
- **Dump** - used by the dump utility to decide when to make a backup, you should just default to 0
- **Pass** - used by `fsck` to decide what order filesystems should be checked, if the value is 0, it will not be checked.

## swap

```
(parted) print
Model: Msft Virtual Disk (scsi)
Disk /dev/sdc: 1100GB
Sector size (logical/physical): 512B/4096B
Partition Table: loop
Disk Flags:

Number  Start  End     Size    File system  Flags
 1      0.00B  1100GB  1100GB  ext4
```

swap is what we use to allocate virtual memory to your system.

If you are low on memory, the system uses this partition to 'swap' pieces of memory of idle processes to the disk, so you're not bogged for memory.

### Using a partiition for swap space

1. First make sure we don't have anything on the partition
2. Run: `mkswap /dev/sdb2` to initialize the swap areas
3. Run: `swapon /dev/sdv2` to enable the swap device
4. If you want the swap partition to persist on bootup, you need to add an entry to the `/etc/fstab` file. `sw` is the filesystem type that you'll use.
5. To remove swap: `swapoff /dev/sdb2`

> Generally you should allocate about twice as much swap space as you have memory. But modern systems today are usually pretty powerful enough and have enough RAM as it is.
