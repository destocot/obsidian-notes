---
title: "Command Line 2"
sidebar:
  order: 2
---

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
