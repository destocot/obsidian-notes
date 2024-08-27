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
