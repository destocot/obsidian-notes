---
title: "Command Line"
sidebar:
  order: 1
---

## the shell

The shell is basically a program that takes your commands from the keyboard and sends them to the operating system to perform.

**bash** (Bourne Again shell)

```bash
echo Hello World
# Output: Hello World
```

```bash
date
# Output: Mon Aug  5 08:35:32 EDT 2024
```

```bash
whoami
# Output: destocot
```

## pwd (Print Working Directory)

Everything in Linux is a file.

The first directory in the filysystem is aptly named the root directory `\`.

```bash
pwd
# Output: /home/destocot
```

## cd (Change Directory)

```
/
|-- home
|   `-- destocot
|       `-- Documents
|           |-- notes.md
```

### absolute path for `notes.md` (from `/home/destocot`)

```bash
/home/destocot/Documents/notes.md
```

### relative path for `notes.md` (from `/home/destocot`)

```bash
Documents/notes.md
# ./Documents/notes.md
```

### changing directories

```bash
cd /home/destocot
# takes you to `/home/destocot` directory

cd Documents
# takes you to `Documents` directory (relative to `/home/destocot`)
```

```bash
cd .
# takes you to current directory

cd ..
# takes you to directory above current directory

cd ~
# takes you to home directory `/home/destocot`

cd -
# takes you to previous directory
```

## ls (List Directories)

```bash
ls
# list directories and files in the current directory

ls /home/destocot
# list directories and files in `/home/destocot`
```

```bash
ls -a
# -a flag (all) lists all directories and files (including hidden files)
```

```bash
ls -l
# -l flag (long) shows a detailed list of directories and files in long format
# Output:
# total 156
# -rw-r--r--  1 destocot destocot   1383 Aug  1 21:24 README.md
# -rw-r--r--  1 destocot destocot    345 Aug  5 21:29 components.json
# -rw-r--r--  1 destocot destocot    201 Aug  1 21:24 next-env.d.ts
# -rw-r--r--  1 destocot destocot     92 Aug  1 21:24 next.config.mjs
# drwxr-xr-x 12 destocot destocot   4096 Aug  5 22:19 node_modules
# -rw-r--r--  1 destocot destocot    908 Aug  5 22:19 package.json
```

> From left: file permissions, number of links, owner name, owner group, file size, timestamp of last modification, and file/directory name

```bash
ls -la
# lists all (-a) directories and files in long (-l) format
```
