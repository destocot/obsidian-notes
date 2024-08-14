---
title: "Advanced Text Fu"
sidebar:
  order: 3
---

## regex (Regular Expressions)

```
sally sells seashells
by the seashore
```

```py
/^by/
# Beginning of a line with ^
# would match the line "by the seashore"
```

```py
/seashore$/
# End of a line with $
# would match the line "by the seashore"
```

```py
/b./
# Matching any single character with .
# would match by
```

```py
/d[iou]g/
# Bracket notation with []
# would match: dig, dog, dug

/d[^i]g/
# anchor tag ^ in bracket means anything except
# would match: dog and dug but NOT dig

/d[a-c]g/
# brackets can also use ranges
# will match pattens like dag, dbg, dcg

/d[A-C]g/
# brackets are case sensitive
# will match dAg, dBg, and dCg but not dag, dbg, and dcg
```

## Vim (Vi Improved)

Vim stands for vi (Improved)

```bash
vim
# fire up vim
```

### Vim Search Patterns

To search for an expresssion just type <kbd>/</kbd> key and then your search result while you are in a vim session.

Once you hit enter, you can press "n" to go forward or "N" to go backward in your search results.

finds first matching string for 'fly' in file

```
/fly
```

find last matching string for 'fly' in file

```
?fly
```

### Vim Navigation

- <kbd>h</kbd> or <kbd>←</kbd> - will move you left one character
- <kbd>k</kbd> or <kbd>↑</kbd> - will move you up one line
- <kbd>j</kbd> or <kbd>↓</kbd> - will move you down one line
- <kbd>l</kbd> or <kbd>→</kbd> - will move you right one character

### Vim Appending Text

To insert text you'll need to enter **INSERT** mode. By default, Vim puts you in **COMMAND** mode.

- <kbd>i<kbd> - insert text before the cursor
- <kbd>O<kbd> - insert text on the previous line
- <kbd>o<kbd> - insert text on the next line
- <kbd>a<kbd> - append text after the cursor
- <kbd>A<kbd> - append text at the end of the line

To exit **INSERT** mode just hit the <kbd>ESC</kbd> key.

### Vim Editing

- <kbd>x</kbd> - used to cut the selected text also used for deleting characters
- <kbd>dd</kbd> - used to delete the current line
- <kbd>y</kbd> - yank or copy whatever is selected
- <kbd>yy</kbd> - yank or copy the current line
- <kbd>p</kbd> - paste the copied text before the cursor

### Vim Saving and Exiting

- <kbd>:w</kbd> - writes or saves the file
- <kbd>:q</kbd> - quit out of vim
- <kbd>:wq</kbd> - write and then quit
- <kbd>:q!</kbd> - quit of vim without saving the file
- <kbd>ZZ</kbd> - equivalent of :wq, but one character faster

- <kbd>u</kbd> - undo your last action
- <kbd>CTRL</kbd> + <kbd>r</kbd> - redo your last action
