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
# -rw-r--r--  1 destocot destocot 112225 Aug  5 22:19 pnpm-lock.yaml
# -rw-r--r--  1 destocot destocot    135 Aug  1 21:24 postcss.config.mjs
# drwxr-xr-x  2 destocot destocot   4096 Aug  1 21:39 public
# drwxr-xr-x  7 destocot destocot   4096 Aug  5 22:11 src
# -rw-r--r--  1 destocot destocot   2179 Aug  5 21:29 tailwind.config.ts
# -rw-r--r--  1 destocot destocot    578 Aug  1 21:24 tsconfig.json
```

> From left: file permissions, number of links, owner name, owner group, file size, timestamp of last modification, and file/directory name

```bash
ls -la
# lists all (-a) directories and files in long (-l) format
```

## touch

`touch` allows you to create new empty files

```bash
touch new_file
```

> `touch` is also used to change timestamps on existing files and directories

## file

`file` allows you to see a description of the file's contents

```bash
file new_file.txt
# Output: new_file.txt: ASCII text
```

## cat

`cat` (short for concatenate) displays file contents

```bash
cat new_file.txt
# Output: asdf1234

cat new_file.txt to_remove.txt
# Output:
# asdf1234
# Lorem ipsum dolor sit amet.
```

## less

`less` allows you to display file contetns in a paged manner

```bash
less long_file.txt
```

### navigating through less

- **q** - Used to quit out of less
- **Page Up / Page Down** - Forward/Backward one window
- **Up / Down** - Forward/Backward one line
- **g** - Moves to beginning of the text file
- **G** - Moves to the end of the text file
- **/<query>** - Search specific text inside the text document.
- **h** - Help about how to use `less` (while in `less`)

## history

```bash
history
# lists history of the commands that you previously entered

!!
# run the previous command that was entered
```

### reverse seach command

- **<kbd>CTRL</kbd> + <kbd>R</kbd>** - enters the `(reverse-i-search)` mode, allowing you to types parts of a command and show you matches

### clear

```bash
clear
# clears the terminal screen
```
