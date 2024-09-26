---
title: "Boot the System"
sidebar:
  order: 17
---

## Boot Process Overview

The Linux boot process can be broken down in 4 simple stages:

1. BIOS - stands for "Basic Input/Output System", initalizs the hardware and makes sure with a Power-on self test (POST) that all the hardware is good to go

2. Bootloader - loads the kernel into memory and then starts the kernel with a set of kernel parameters

> GRUB is one of the most common bootloaders, which is a universal Linux standard.

3. Kernel - initalizes devices and memory

4. Init - starts and stops essential service process on the system

## Boot Process: BIOS

The BIOS's main goal is to find the system bootloader.

It searches the boot block to figure out how to boot the system. Depending on how you partition your disk, it will look at the master boot record (MBR) or GPT.

The MBR contains the code to load another program somewhere on the disk, this program loads up our bootloader.

### UEFI

An alternative to using BIOS, UEFI (Unified extensible firmware interface).

UEFI was designed to be a successor to BIOS.

UEFI stores all information about startup in an `.efl` file.

## Boot Process: Bootloader

The bootloader's main responsibilities are:

- Booting into an operating system, it can be used to boot to non-Linux operating systems
- Select a kernel to use
- Specify kernel parameters

The kernel parameters (which determins where the bootloader finds the kernel) can be accessed by the GRUB menu on startup using the **e** key.

- initrd - Specifies the location of initial RAM disk
- BOOT_IMAGE - where the kernel image is located
- root - location of the root file system, the kernel searches inside this location to find init. It is often represented by its **UUID** or the device name such as `/dev/sda1`.
- ro - mounts the filesystem as read-only mode
- quiet - hides display messages that are going on in the background during boot
- splash - lets splash screen be shown

## Boot Process: Kernel

### initrd vs initramfs

- in older versions of Linux, initrd (initial ram disk) would be mounted an dload the necessary bootup drivers that would be replaced once everything was loaded

- these days, initramfs, is a temporary root filesystem that is built into the kernel itself to load all necessary drivers for the real root system, (no more locating the initrd file)

### Mounting the root filesystem

First the root partition is mounted in read-only mode so that `fsck` can run safely and check for system integrity. Afterwards it remounds the root filesystem in read-write mode.

The kernel locates the init program and enables it.

## Boot Process: Init

- **System V init (sysv)** - traditional init system

  - sequentially starts and stops processes, based on startup scripts
  - the state of the machine is denoted by runlevels, each runlevel starts or stops a machine in a different way.

- **Upstart** - init you'll find on older Ubuntu installations

  - uses the idea of jobs and events and works by starting jobs that perform certain actions in response to events.

- **Systemd** - new standard for init, goal oriented
  - systemd tries to satisfy the goal's dependencies when their is a goal to be achieved
