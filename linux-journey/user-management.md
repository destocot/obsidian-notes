---
title: "User Management"
sidebar:
  order: 8
---

## Users and Groups

The system uses

- user IDs (UID) to manage users.
- groups to manage permissions, groups are just sets of users with permission set by that group, they are identified with their group ID (GID).

The sudo command (superuser do) is used to run a command with root access.

```bash
cat /etc/shadow
# cat: /etc/shadow: Permission denied
```

```bash
ls -la /etc/shadow
-rw-r----- 1 root shadow 831 Jul 22 02:25 /etc/shadow
# root is the owner of the file

sudo cat /etc/shadow
# run `cat /etc/shadow` with root access
```

> in order to access `/etc/shadow` you'll need root access to be part of the shadow group

## root

```bash
su
# will "substitute users" and open a root shell if no username is specified
```

There is a file called the `/etc/sudoers` file, this file list users who can run sudo. You can edit this file with the **visudo** command.

## /etc/passwd

```bash
cat /etc/passwd
# find out what users are mapped to what user ID
```

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
...
destocot:x:1000:1000:,,,:/home/destocot:/bin/bash
```

Fields are separated by colons

1. Username
2. User's password

- "x" means the password is stored in the `/etc/shadow` file
- "\*" means the user doesn't have login accesss
- " " blank field means the user doesn't have a password.

3. The user ID (e.g. destocot has the UID of 1000)
4. The group ID
5. GECOS field - generally users to leave comments about the user or account such as their real name or phone number, it is comma delimited.
6. User's home directory
7. User's shell

> You can edit the `/etc/passwd` file by hand if you want to add users and modify information with the **vipw** tool. However, it is better to use tools such as `useradd` and `userdel`.

## /etc/shadow

```bash
cat /etc/shadow
# user to store information about user authentication
```

```
root:*:19683:0:99999:7:::
daemon:*:19683:0:99999:7:::
bin:*:19683:0:99999:7:::
sys:*:19683:0:99999:7:::
...
destocot:$y$j9T$H3PvdvTlHBsposbC/qSvt.$hp...:19926:0:99999:7:::
```

Fields are separated by colons

1. Username
2. Encrypted password
3. Date of last password changed - expressed as the number of days since Jan 1, 1970. If there is a `0` that means the user should change their password the next time they login
4. Minimum password age - days that a user will have to wait before being able to change their password
5. Maximum password age - maximum number of days before a user has to change their password
6. Password warning period - number of days before a password is going to expire
7. Password inactivity period - number of days after a password has expires to allow login with their password
8. Account expiration date - date that usre will not be able to login
9. Reserved field for future use

> In most distributions today, user authentication doesn't rely on just the `/etc/shadow` file, there are other mechanisms in place such as PAM (Pluggable Authentciation Modules) that replace authentication

## /etc/group

```bash
cat /etc/group
# user for user management, allows for different groups with differnet permissions
```

```
root:x:0:
daemon:x:1:
...
adm:x:4:syslog,destocot
...
destocot:x:1000:
```

Fields are separated by colons

1. Group name
2. Group password - there isn't a need to set a group password, useing an elevated privilege like sudo is standard, A "\*" will be put in place as the default value.
3. Group ID (GID)
4. List of users - you can manually specify usres you want in a specific group

## User management tools

### Adding Users

```bash
useradd janniknoa
# creates an entry in `/etc/passwd` and `/etc/shadow, sets up default groups

adduser janniknoa
# contains more helpful features than `useradd` such as making a home directory
```

> `useradd` will create a user without a home directory or password (you will need to set up these manually)

### Removing Users

```bash
userdel janniknoa
# removes user

userdel -r janniknoa
# the -r (remove) flag also removes the home directory and mail spool
```

### Changing passwords

```bash
passwd janniknoa
# allows you to change the password of yourself or another user (if you are root)
```
