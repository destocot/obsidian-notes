---
title: "Text-Fu 3"
sidebar:
  order: 6
---

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
