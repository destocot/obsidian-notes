---
title: "The Filesystem"
sidebar:
  order: 14
---

## Filesystem Hierarchy

`ls -l /` will list all directories under the root directory

- `/` - The root directory of the entire filesystem hierarchy, everything is nestled under this directory.
- `/bin` - Essential ready-to-run programs (binaries), includes the most basic commands such as ls and cp.
- `/boot` - Contains kernel boot loader files.
- `/dev` - Device files.
- `/etc` - Core system configuration directory, should hold only configuration files and not any binaries.
- `/home` - Personal directories for users, holds your documents, files, settings, etc.
- `/lib` - Holds library files that binaries can use.
- `/media` - Used as an attachment point for removable media like USB drives.
- `/mnt` - Temporarily mounted filesystems.
- `/opt` - Optional application software packages.
- `/proc` - Information about currently running processes.
- `/root` - The root user's home directory.
- `/run` - Information about the running system since the last boot.
- `/sbin` - Contains essential system binaries, usually can only be ran by root.
- `/srv` - Site-specific data which are served by the system.
- `/tmp` - Storage for temporary files
- `/usr` - This is unfortunately named, most often it does not contain user files in the sense of a home folder. This is meant for user installed software and utilities, however that is not to say you can't add personal directories in there. Inside this directory are sub-directories for /usr/bin, /usr/local, etc.
- `/var` - Variable directory, it's used for system logging, user tracking, caches, etc. Basically anything that is subject to change all the time.

## Filesystem Types

Since there are so many different filesystem implementations, we utilize the Virtual File System (VFS) abstraction layer. It is a layer between applications and the different filesystem types, so no matter what filesystem you have, your applications will be able to work with it.

### Journaling

If you are on a non-journaled filesystem, the file would end up corrupted and your filesystem would be inconsistent and then when you boot back up, your system would perform a filesystem check to make sure everything is ok.

On a journaled system, before your machine even begins to copy the file, it will write what you're going to be doing in a log file (journal). The filesystem is always in a consistent state because of this, so it will know exactly where you left off if your machine shutodwn suddenly.

### Common Desktop Filesystem Types

- **ext4** - the most current version of the native Linux filesystems
- **Btrfs** (Better or Butter FS) - a new filesystem for Linux that comes with snapshots, incremental backups, performance increase and much more
- **XFS** - High performance journaling file system
- **NTFS** and **FAT** - windows filesystems
- **HFS+** - Macintosh fileystems

```bash
df -T
# check whick filesystems are on your machine
# the -T (print-type) flag prints the file system type
```

The `df` command reports file system disk space usage an dother details about your disk.

## Anatomy of a Disk

Hard disks can be subdivided into partitions, essentially making multiple block devices.

### Partition Table

Every disk will have a partition table, this table tells the system how the disk is partitioned.

There are two main partition table schemes used, Master Boot Record (MBR) and GUID Partition Table (GPT).

### Partition

- **MBR** - Traditional partition table, can have primary, extended, and logical partitions.

- **GPT** -- New standard for disk partitioning, has only one type of partition and you can make many of them.

## Filesystem Structure

- Boot block - contains information used to boot the operating system.

- Super block - comes after the **Boot block**, contains information about the filesystem, such as the inode table, size of the logical blocks and the size of the filesystem.

- Inode table - think of this as the database that manages our files. Each file or directory has a unique entry in the inode table and it has various information about the file.

- Data blocks - this is the actual data for the files and directories.

## Disk Partitioning
