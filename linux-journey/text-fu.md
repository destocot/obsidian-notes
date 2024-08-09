---
title: "Text-Fu"
sidebar:
  order: 2
---

## Overview

### Key Terms

- I/O - input/output
- stdin - standard input
- stdout - standard output

Processes uses I/O streams to receive input and return output.

## stdout (Standard Out)

```bash
echo Hello World > peanuts.txt
```

- `echo Hello World` - by default the `echo` command takes the input (stdin) from the keyboard and returns the output (stdout) to the screen.

- `>` - is a redirection operator that allow us to change where standard output goes. It allows us to send the output of `echo Hello world` to a file instead of the screen.

> If the file does not exist it will create it. However, if the file exists we will overwrite it.

```bash
echo Hello World >> peanuts.txt
```

- `>>` - is a redirection operator that allows us to **append** to the end of the `peanuts.txt` file instead of overwrite it.

## stdin (Standard In)

```bash
cat < peanuts.txt > banana.txt
```

- `<` can be used for stdin redirection.

- We redirect `peanuts.txt` to be our stdin. Then the output of `cat peanuts.txt` is redirected to another file called `banana.txt`

## stderr (Standard Error)

```bash
ls /fake/directory > peanuts.txt
# ls: cannot access '/fake/directory': No such file or directory
```

- The standard error (stderr) is another I/O stream. By default, stderr sends its output to the screen.

### File Descriptors

A file descriptor is a non-negative number that is used to accesss a file or stream.

stdout - 0
stdin - 1
stderr - 2

```bash
ls /fake/directory 2> peanuts.txt
# redirect stderr to `peanuts.txt` file
```

```bash
ls /fake/directory > peanuts.txt 2>&1
# redirect both stdout and stderr to `peanuts.txt`

ls /fake/directory &> peanuts.txt
# (shorthand) redirect tdout and stderr to `peanuts.txt`
```

```bash
ls /fake/directory 2> /dev/null
# redirect stdin to a special file called `/dev/null` whcih will discard any input
```

## pipe and tee

### pipe

The pipe operator `|`, allows us to get the stdout of a command and make that the stdin to another process.

```bash
ls -la /etc | less
# pipe the stdout of `ls -la /etc` as the stdin to the `less` command
```

### tee

```bash
ls | tee peanuts.txt
# will output the stdout to the screen AND redirect it to `peanuts.txt`
```
