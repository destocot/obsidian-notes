---
title: "The Filesystem 3"
sidebar:
  order: 16
---

## Disk Usage

```bash
df -h
# The df command shows you the utilization of your currently mounted filesystems.
# The -h (human-readable) flag, gives you a human readable format.
```

```bash
du -h
# The du command shows you the disk usage of the current directory you are in.
# The -h (human-readable) flag, gives you a human readable format.
```

## Filesystem Repair

The `fsck` (filesystem check) command is used to check the consistency of a filesystem and can even try to repair to for us.

Usuallly, when you boot up a disk `fsck` will run before your disk is mounted to make sure everything is ok.

```bash
sudo fsck /dev/sda
```

## Inodes

### What is an inode?

An inode (index node) is an entry in the inode table (the database that manages the files in our filesystem).

There is one for every file and it describes everything about the file, such as:

- File type - regular file, directory, character device, etc
- Owner
- Group
- Access permissions
- Timestamps - mtime (time of last file modification), ctime (time of last attribute change), atime (time of last access)
- Number of hardlinks to the file
- Size of the file
- Number of blocks allocated to the file
- Pointers to the data blocks of the file (Most Important!)

### Where are inodes created?

When a filesystem is created, space for inodes is allocated as well.

Data storage depends on both the data and the database (inodes).

```bash
df -i
# The -i (inodes) flag lists inode information instead of block usage
```

### Inode information

Inodes are identifies by numbers, when a file gets created it is assigned an inode number.

```bash
ls -li
# the -i (inode) flag shows the index number of each file
```

```
 32453 drwxr-xr-x  3 destocot destocot 4096 Aug 28 11:52 Documents
120849 drwxr-xr-x  4 destocot destocot 4096 Aug 14 14:51 archive
```

The first number in the command is the inode number.

You can also see detailed information about a file with stat.

```bash
stat Documents/
```

```
  File: Documents/
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: 820h/2080d      Inode: 32453       Links: 3
Access: (0755/drwxr-xr-x)  Uid: ( 1000/destocot)   Gid: ( 1000/destocot)
Access: 2024-09-25 09:06:06.483010215 -0400
Modify: 2024-08-28 11:52:32.077370283 -0400
Change: 2024-08-28 11:52:32.077370283 -0400
 Birth: 2024-08-14 12:59:55.302810858 -0400
```

### How do inodes locate files?

Inodes point to the actual data blocks of your files. In a typical filesystem (not all work the same), each inode contains 15 pointers, the first 12 pointers point directly to the data blocks.

## symlinks

```bash
ls -li
# the -i (inode) flag shows the index number of each file
```

```
 32453 drwxr-xr-x  3 destocot destocot 4096 Aug 28 11:52 Documents
120849 drwxr-xr-x  4 destocot destocot 4096 Aug 14 14:51 archive
```

The thid number in the command is the link count. The link count is the number of hard links a file has.

### Symlinks

In Linux, the equivalnet of a shortcut (in Windows) are symbolic links (or soft links or symlinks). Symlinks allow us to link to another file by its filename.

```bash
echo 'myfile' > myfile
echo 'myfile2' > myfile2
echo 'myfile2' > myfile3

ln -s myfile myfilelink
ls -li
```

```
total 12
5449 -rw-r--r-- 1 destocot destocot 7 Sep 25 09:59 myfile
5772 -rw-r--r-- 1 destocot destocot 8 Sep 25 09:59 myfile2
6171 -rw-r--r-- 1 destocot destocot 8 Sep 25 09:59 myfile3
6178 lrwxrwxrwx 1 destocot destocot 6 Sep 25 10:00 myfilelink -> myfile
```

Here we create a symbolic link named myfilelink that points to myfile. Symbolic links are denoted by `->`.

Notice how you get a new inode number, symlinks are just files that point to filenames.

If you use a symlinks they do not use inode numbers, they use filenames, so they can be references across different filesystems.

### Hardlinks

```bash
ln myfile2 hardlink
ls -li
```

```
total 16
5449 -rw-r--r-- 1 destocot destocot 7 Sep 25 09:59 myfile
5772 -rw-r--r-- 2 destocot destocot 8 Sep 25 09:59 myfile2
6171 -rw-r--r-- 1 destocot destocot 8 Sep 25 09:59 myfile3
6178 lrwxrwxrwx 1 destocot destocot 6 Sep 25 10:00 myfilelink -> myfile
5772 -rw-r--r-- 2 destocot destocot 8 Sep 25 09:59 myhardlink
```

A hardlink just creates another file with a link to the same inode. So if I modified the contents of myfile2 or myhardlink, the change would be seen on both.

However, if I deleted myfile2, the file would still be accessible through myhardlink.

This is where the link count comes into play, the link count is the number of **hard links** that an inode has.

### Creating a symlink

```bash
ln -s myfile mylink
```

### Creating a hardlink

```bash
ln somefile somelink
```
