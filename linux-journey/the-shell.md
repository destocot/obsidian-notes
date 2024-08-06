---
title: "The Shell"
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

## change directories

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
