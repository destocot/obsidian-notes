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

## env (Environment)

```bash
echo $HOME
# prints home directory `/home/destocot`

echo $USER
# prints username `destocot`
```

```bash
env
# prints all environment variables
```

### PATH variable

The `$PATH` variable is responsible for the directories where you can run binaries.

## cut

```bash
cut -c5 sample.txt
# outputs the 5th character in each line of the file

cut -f2 sample.txt
# the -f (field) flag cuts text based off of fields
# by default the delimiter is TAB and select the second field

cut -f1 -d ';' sample.txt
# we change the delimiter to `;` and select the first field
```

## paste

```bash
paste -s sample2.txt
# merges lines in `sample2.txt` together
# by default the delimiter is TAB

paste -d ' ' -s sample2.txt
# merges lines in `sample2.txt` together
# we change the delimiter to a space ' '
```

## head

```bash
head /var/log/syslog
# prints the first 10 lines (default) of `/var/log/syslog`

head -n15 /var/log/syslog
# prints the first 15 lines of /var/log/syslog
```

## tail

```bash
tail /var/log/syslog
# prints the last 10 lines (default) of `/var/log/syslog`

tail -n5 /var/log/syslog
# prints the last 5 lines of `/var/log/syslog`

tail -f /var/log/syslog
# the -f (follow) flag will show changes that get added to the file
```

## expand and unexpand

### expand

```bash
expand sample.txt > result.txt
# redirects contents of `sample.txt` which each TAB converted to a group of spaces
```

### unexpand

```bash
unexpand -a result.txt
# converts each group of spaces to a TAB
```

## join and split

### join

```bash
join file1.txt file2.txt
# will join both files by the first field (default)

join -1 2 -2 1 file1.txt file2.txt
# will join both files by the second file of file 1 (-1)
# and the first field of file 2 (-2)
```

### split

```bash
split notes.txt
# will split a file into different files for each 1000 lines (default)
# each file will be named `x**` by default

split --lines 2 notes.txt
# will split a file into different files for each 2 lines
```

## sort
