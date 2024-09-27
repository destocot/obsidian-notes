---
title: "Kernel"
sidebar:
  order: 18
---

## Overview of the Kernel

The Linux operating system can be organized into thee different levels of abstraction.

- hardware - this includes our CPU, memory, hard disks, networking ports, etc.

> physical layer that actually computers what our machine is doing

- kernel - handles process and memory management

> talks to the hardware to make sure it does what we want our processes to do

- user space - the shell, the programs you run, the graphics, etc.

## Privilege Levels

kernel operates in kernel mode

- has complete access to the hardware, it controls everything

user space operates in user mode

- has access to a very small amount of safe memory and CPU

## System Calls

System calls (syscall) provide user space processes a way to request the kernel to do something for us.

The kernel makes certain services available through the system call API.

Your system already has a table of what system calls exist and each system call has a unique ID.

```bash
# view the system calls that a process makes
strace ls
```

## Kernel Installation

```bash
# kernel version you have on your system
uname -r
# uname prints system information
# the -r (kernel-release) flag prints the kernel release
```

```
5.15.153.1-microsoft-standard-WSL2
```

You can install the Linux kernel in differnet ways, you can download the source package and compile from source or you can install it using package management tools.

If you want to update kernel version, just use `dist-upgrade`.

```bash
sudo apt dist-upgrade
```

## Kernel Location

When you install a new kernel it adds a couple of files to your system, these files are usually added to the `/boot` directory.

- **vmlinuz** - the actual linux kernel
- **initrd** - temporary file system, used before loading the kernel
- **System.map** - symbolic lookup table
- **config** - kernel configuration settings

## Kernel Modules

Kernel modules are pieces of code that can be loaded and unloaded into the kernel on demand.

```bash
# view a list of currently loaded modules
lsmod

# load a module
sudo modprobe bluetooth

# remove a module
sudo modprobe -r bluetooth
```

### Load on bootup

You can also load modules on system boot. Just modify the `/etc/modprobe.d` directory and add a configuration file in it like so:

```
pete@icebox:~$ /etc/modprobe.d/peanutbutter.conf

options peanut_butter type=almond
```

### Do not load on bootup

```
pete@icebox:~$ /etc/modprobe.d/peanutbutter.conf

blacklist peanut_butter
```
