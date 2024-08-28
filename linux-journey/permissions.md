---
title: "Permissions"
sidebar:
  order: 5
---

## File Permissions

```bash
ls -l Documents/
# drwxr-xr-x 2 destocot destocot 4096 Aug 27 11:21 lyrics
```

There are four parts to a file's permissions. The first part is the **filetype**, which is denoted by the first character in the permisions.

```
d | rwx | r-x | r-x
```

In this case the `d` represents a directory. Most commonly you will see a `-` for a regular file.

The next three parts are the actual permissions. THe first 3 bits are **user permissions**, then **group permissions**, and then **other permissions**.

- `r` - readable
- `w` - writeable
- `x` - executable
- `-` - empty

In the above example we can see:

- user `destocot` have read, write, and execute permissions on the file
- group `destocot` have read and execute permissions
- other users, have read and execute permissions

## Modifying Permissions

Changing permissions can be done with the **chmod** command.

```bash
chmod u+x myfile
# add executable permission bit on the user set

chmod u-x myfile
# remove executable permission bit on the user set

chmod ug+w myfile
# add write permission bit on the user and group set
```

### Numerical Format

- 4 - read permission
- 2 - write permission
- 1 - execute permission

```bash
chmod 755 myfile
# 7 represents user's permissions (4 + 2 + 1) gives read, write, and execute permissions
# 5 represents group's permissions (4 + 1) gives read and execute permissions
# 5 represents all other user's permissions (4 + 1) gives read and execute permissions
```

## Ownership Permissions

```bash
sudo chown patty myfile
# set the owner of the myfile to patty
```

```bash
sudo chgrp whales myfile
# set the group of myfiles to whales
```

```bash
sudo chown patty:whales myfile
# set both the user and group at the same time
```

## Umask

Every file that gets created comes with a default set of permissions. We can use the **unmask** command to change the default set of permissions.

> **unmask** takes away permissions

```bash
umask 021
# allow users to access everything
# take away write permissions for groups
# take away executable permissions fo rother users
```

> The default **umask** command is `022` meaning full user access, but no write access for group and other users.

## Setuid

The Set User ID (SUID) allows a user to run a program as the owner of the program file rather than as themselves.

```bash
passwd
# the passwd command modifies the /etc/shadow file

ls -l /etc/shadow
# -rw-r----- 1 root shadow 831 Aug 16 11:45 /etc/shadow
# the /etc/shadow file is owned by root
```

We can look at the permission set on the `passwd` command

```bash
ls -l /usr/bin/passwd
# -rwsr-xr-x 1 root root 59976 Feb  6  2024 /usr/bin/passwd
```

We can see that the user set has a permission bit **s**,

The **s** permission bit is the **SUID**, it allows the users who launch the program to get the file owner's permission as well as execution permiossion (in this case **root**).

### Modifying SUID

```bash
sudo chmod u+s myfile
# symbolic way

sydo chmod 4755 myfile
# numerical way
```

The **SUID** is denoted by a 4 and pre-prended to the permission set. If the **SUID** is denoted with a capital **S** that means it still does the same thing, but it does not have execute permissions.

## Setgid

Similiar to **SUID**, there is a set group ID (**SGID**) permission bit.

```bash
ls -l /usr/bin/crontab
# -rwxr-sr-x 1 root crontab    39568 Mar 23  2022 crontab
```

The `crontab` program has permissions to run as the `crontab` group.

### Modifying SGID

```bash
sudo chmod g+s myfile

sudo chmod 2555 myfile
```

The numerical represntation for **SGID** is **1**.

## Process Permissions

There are **three** UIDS associated with every process.

1. **effective user ID**: The permissions of the user or group that launched the processs.

2. **real user ID**: The ID of the user that launched the process.

3. **saved user ID**: Allows a process to switch between the effective UID and real UID, vice versa.

### Example

Let's say your effective user ID is 500, when you run `passwd` you will have `SUID` permissions enabled. This will let the `passwd` process havce access to both your UID (500) and the root user's ID (0).

## The Sticky Bit

This permission bit, "sticks a file/directory" this means that only the owner or the root user can delete or modify the file. (Useful for shared directories)

```bash
ls -ld /tmp/
# drwxrwxrwt 10 root root 4096 Aug 28 11:29 /tmp/
```

The special permission bit at the end **t**, means everyone can add files, write files, and modify files in the `/tmp` directory, but only the root can delete the `/tmp` directory.

### Modify sticky bit

```bash
sudo chmod +t mydir

sudo chmod 1755 mydir
```

The numerical representation for the sticky bit is **1**.
