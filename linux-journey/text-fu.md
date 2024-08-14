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

```bash
sort file1.txt
# sorts lines alphabetically

sort -r file1.txt
# the -r (reverse) flag sorts lines reverse alphabetically

sort -n file1.txt
# the -n (numeric-sort) sorts lines numerically
```

## tr (Translate)

```bash
tr a-z A-Z
# enters a prompt to translate lowercase to uppercase characters
```

## uniq (Unique)

```bash
uniq reading.txt
# prints out lines without duplicates

uniq -c reading.txt
# the -c (count) flag counts the occurrences of each line

uniq -u reading.txt
# the -u (unique) flag only shows unique lines

uniq -d reading.txt
# the -d (repeated) flag only shows duplicated lines
```

> `uniq` does not detect duplicate lines unless they are adjacent. We can overcome this issue by using `uniq` in combination with `sort`

```bash
sort reading.txt | uniq
# sorts files alphabetically (making them adjacent) then prints all lines without duplicates
```

## wc and nl

```bash
wc /etc/passwd
# displays the number of lines, number of words, number of bytes, respectively

wc -l /etc/passwd
# displays number of lines

wc -w /etc/passwd
# displays number of words

wc -c /etc/passwd
# displays number of bytes
```

```bash
nl file1.txt
# displays the line number next to each line

nl -s '. ' file1.txt
# displays the line line number appeneded with a '. ' next to each line
```

## grep

```bash
grep fox sample.txt
# searches for string 'fox' in sample.txt

grep -i "example" sample.txt
# the -i (ignore-case) flag searchs for string 'example' (case insensitive) in sample.txt

env | grep -i User
# pipes the env (Environment) to grep and searches for the string 'User' (case insensitive)

ls /home/destocot/Documents | grep '.txt$'
# grep can use regular expressions
# returns all files ending with '.txt' in '/home/destocot/Documents'
```
