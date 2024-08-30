---
title: "Command Line 3"
sidebar:
  order: 3
---

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

## find

```bash
find /home -name puppies.jpg
# search in `/home` directory for file by the name of `puppies.jpg`

find /home -type d -name MyFold
# search in `/home` directory for file of type `directory` and name of `puppies.jpg`
```

## help

The `help` command is a built-in bash command that provides help for other bash commands

```bash
help pwd
# gives you a description and the options you can use when you want to run pwd
```

For executable programs it is a convention to have an option called `--help` or something similar.

```bash
pwd --help
# --help flag gives description and options you can use when you want to run pwd
```

> Not all developers who ship out executables will conform to this standard.

## man (Manual)

```bash
man ls
# gives the manual for the ls command
```

Man pages are manuals that are by default build into most Linux operating systems.

## whatis

```bash
whatis cat
# provides a brief description of the cat command
```

> The description gets sourced from the manual page of each command.

## alias

We can use an alias to reference repetitive and/or long commands.

```bash
alias foobar='ls -la'
# we create an alias called `foobar` which runs the command `ls -la`
```

> aliases are not saved after reboot, to permanently add an alias we need to place it in our `~/.bashrc` or similar file

```bash
unalias foobar
# removes the alias foobar
```

## exit

```bash
exit
# exit from shell
```

```bash
logout
# exit from shell
```
