---
title: "Devices"
sidebar:
  order: 13
---

## /dev directory

When you connect a _device_ to your machine, it generally needs a _device driver_ to function properly.

The device files are generally stored in the `/dev` directory.

```bash
ls /dev
```

## device types

```bash
ls -l /dev
```

```
total 0
brw-rw----   1 root disk      8,   0 Dec 20 20:13 sda
crw-rw-rw-   1 root root      1,   3 Dec 20 20:13 null
srw-rw-rw-   1 root root           0 Dec 20 20:13 log
prw-r--r--   1 root root           0 Dec 20 20:13 fdata
```

The columns are as follows from left to right:

- Permissions
- Owner
- Group
- Major Device Number
- Minor Device Number
- Timestamp
- Device Name

Device files are denoted as the following:

### c - character

- these devices transfer data, but one character at a time.
- these devices aren't really physically connected to the machine, but thye allow the operating system greater functionality

### b - block

- these devices transfer data, but in large fixed-sized blocks

**examples**: hard drives, filesystems, etc.

### p - pipe

- named pipes allow two or more process to communicate with each other
- similar to character devices, but instead of having output sent to a device, it's sent to another process

### s - socket

- facilitate communication between processes
- similar to pipe devices but they can communicate with many processes at once

### Device Characterization

Devices are characterized using two numbers, **major device number** and **minor device numbers**.

```
brw-rw----   1 root disk      8,   0 Dec 20 20:13 sda
```

The major device number represents the device driver that is used, in this example 8, which is often the major number for (sd) block devices.

The minor number tells the kernel which unqiue device it is in this driver class, in this case 0 is used to represent the first device (a).

## Device Names

### SCSI Devices

- SCSI ("scuzzy") protocol usually is used by any sort of mass storage on your machine.
- SCSI stands for Small Computer System Interface
- It is a protocol used for allow communication between disks, printers, scanners and other peripherals to your system.

Linux systems correspond SCSI disks with hard disk drives in `/dev`.

- `/dev/sda` - first hard disk
- `/dev/sdb` - second hard disk
- `/dv/sda3` - third partition on the first disk

### Pseudo Devices

Most pseudo devices are character devices.

- `/dev/zero/` - accepts and discards all input, produces a continuous steam of NULL (zero value) bytes
- `/dev/null` - accepts and discards all input, produces no output
- `/dev/random` - produces random numbers

### PATA Devices

Sometimes in older systems you may see hard drives being referred to with an hd prefix:

- `/dev/hda` - first hard disk
- `/dev/hdd2` - second partition on 4th hard disk

## sysfs

sysfs is a virtual filesystem, most often mounted to the `/sys` directory.

- the `/dev` directory allows other programs to access devices themselves
- the `/sys` filesystem is used to view information and manage the device

```
ls /sys/block/sda
alignment_offset  device             events_async       holders   range      slaves
bdi               discard_alignment  events_poll_msecs  inflight  removable  stat
capability        diskseq            ext_range          mq        ro         subsystem
dev               events             hidden             queue     size       uevent
```

## udev

The **udev** system dynamically creates and removes device files for us depending on whether or not they are connected.

- there is a udevd daemon that is running on the system and it listens for messages from the kernel about devices connected to the system.
- udevd will parse that information and it will match the data with the rules that are specifices in `/etc/udev/rules.d`

```bash
udevadm info --query=all --name=/dev/sda
# view the udev database and sysfs using the `udevadm` command.
```

## Isusb, Ispci, Issci

```bash
lsusb
# listing USB devices

lspci
# listing PCI devices

lsscsi
# listing scsi devices
```

## dd

`dd` is a super useful for converting and copying data

```bash
dd if=/home/pete/backup.img of=/dev/sdb bs=1024
# copying the contents of `backup.img` to `/dev/sdb`
# it will copy the data in blocks of 1024 bytes
# until there is no more data to be copied
```

- `if=file` - Input file, read from a file instead of standard input
- `of=file` - Output file, write to a file instead of standard output
- `bs=bytes` - Block size, it reads and writes this many bytes of data at a time.
  - You can use different size metrics by denoting the size with a k for kilobyte, m for megabyte, etc, so 1024 bytes is 1k
- `count=number` - Number of blocks to copy

```bash
dd if=/home/pete/backup.img of=/dev/sdb bs=1M count=2
# we copy only 1M 2 times, leaving only 2M being copied
# `backup.img` is 10M, this leaves our copied data incomplete
```

> `dd` is extremely powerful, you can use it to make backups of anything, including whole disk drives, restoring disk drives, and more.
