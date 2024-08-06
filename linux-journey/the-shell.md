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

## cp (Copy)

```bash
cp new_file.txt /Copies
# copy new_file.txt into `/Copies` directory
```

### wildcards

We can copy multiple files and directories as well using wildcards.

- **\*** - used to represent all single characters or any string
- **?** - used to represent one character
- **[]** - user to represent any character within the brackets

```bash
cp *.txt /Copies
# copies all files with the .txt extension into the `/Copies` directory
```

### recursive

```bash
cp -r Files/ /Copies
# copies all files and directories within the `Files/` directory
```

### -i (interactive flag)

If we copy a file to a directory that already has a file of the same filename, it will be overwritten.

We can avoid this by using the `-i` (interactive flag) to prompt us before overwriting a file.

```bash
cp -i new_file.txt /Copies
```

## mv (Move)

The `mv` (Move) command is used to move and rename files.

```bash
mv old_file new_file
# renames file old_file to new_file

mv old_file /NewFolder
# moves file to `/NewFolder` directory

mv my_file1 my_file2 /NewFolder
# moves multiples files to `/NewFolder` directory

mv NewFolder1/ NewFolder2/
# renames directory NewFolder1/ to NewFolder2/### -i (interactive flag)

mv -i  NewFolder1/ NewFolder2/
# -i flag (interactive) prompts us before overwriting a file (see cp)

mv -b NewFolder1/ NewFolder2/
# creates a backup of the file (prefixed with ~) if it will be overwritten
```

## mkdir (Make Directory)

```bash
mkdir colors
# creates directory colors

mkdir colors sizes
# creates multiple directories colors and sizes

mkdir -p colors/seasons/winter
# -p flag (parents) creates parent directories as needed
```

## rm (Remove)

```bash
rm file1
# removes file
```

> Take caution when using `rm`, there is no trash can that you can fish out removed files. Write-protected files will prompt you for confirmation before deleting them (as well as write-protected directories).

```bash
rm -f file
# -f flag (force) will tell rm to remove all files whether they are write protected or not

rm i file
# -i flag (interactive) prompts us before removing the file (see cp)

rmdir NewFolder/
# remove directory (must be empty)

rm -r NewFolder/
# -r flag (recursive) removes all files and subdirectories in a direcotry
```
