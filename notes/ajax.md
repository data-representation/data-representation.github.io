---
title: Regular Expressions in Go
description: An introduction to using regular expressions in Go
---

# Regular Expressions in Go
[Regular expressions](https://www.regular-expressions.info/) define patterns of characters.
They are commonly used in find and replace operations in text editors, databases and programming languages.
Regular expressions are strings of characters themselves, but those characters represent various parts of patterns.

## A few examples
The regular expression `ab+c` matches any string that begins with an `a`, followed by one or more `b` characters, and ends with a `c`.
So `ab+c` will match all of the following:

- `abc`
- `abbc`
- `abbbc`
- `abbbbbbbbbc`
- `abbbbbbbbbbbbbbbc`

but none of these:

- `ac`
- `bccccc`
- `cow`
- ` `
- `123`