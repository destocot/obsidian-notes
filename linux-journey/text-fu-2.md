---
title: "Text-Fu 2"
sidebar:
  order: 5
---

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
