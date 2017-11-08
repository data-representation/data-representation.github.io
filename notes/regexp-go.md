---
title: Regular Expressions in Go
description: An introduction to using regular expressions in Go
---

# Regular Expressions in Go
[Regular expressions](https://www.regular-expressions.info/) define patterns of characters.
They are commonly used in find and replace operations in text editors, databases and programming languages like [Go](https://golang.org/).
Regular expressions are strings of characters themselves, but those characters represent various parts of patterns.
This post provides a brief introduction, focused on Go.
For more information see [regexone.com](https://regexone.com/) and [regular-expressions.info](https://www.regular-expressions.info).

## An example
The regular expression `ab+c` matches any string that begins with an `a`, followed by one or more `b` characters, and ends with a `c`.
So, `ab+c` will match `abc`, `abbc`, `abbbc`, `abbbbbbbbbc`, and `abbbbbbbbbbbbbbbc`, but not `ac`, `bccccc`, `cow`, or `123`.

The characters `a`, `b` and `c` in the regular expression literally mean the letters a, b and c.
In fact, most literal strings that you might want to search for are valid regular expressions themselves, though they only match a single string - themselves!
However, the `+` has a special meaning - it's a quantifier than means *one or more of those*.
So, `b+` means one or more `b`.

## Uses
The uses of regular expressions are many fold.
One good example is using them to verify email addresses.
Suppose you have a form on your website where visitors can sign up to your mailing list.
You have a text input field where they put their email address.
You can verify that what the visitor has typed a valid email address (no spaces, one `@` symbol, etc.) using a regular expression.
A basic regular expression that will go some of the way is:

```
[^@ ]+@[^@ ]
```

This means at least one character that is not an @ symbol or a space, followed by an @ symbol, followed by at least one character that is not an @ symbol or a space.
Unfortunately, this example is not a very good regular expression for this use as it matches `!"£@""` (amongst infinite others) and that is not a valid email address.
Something like the following will work better.

```
[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}
```


## Syntax
A piece of software that checks if a regular expression matches a string is called an engine.
Regular expression engines are built into editors like [Notepad++](http://docs.notepad-plus-plus.org/index.php/Regular_Expressions), [Visual Studio](https://msdn.microsoft.com/en-us/library/2k3te2cs.aspx) and [Sublime](http://docs.sublimetext.info/en/latest/search_and_replace/search_and_replace_overview.html).
They also are provided in the standard libraries of [Go](https://golang.org/pkg/regexp/), [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), [Python](https://docs.python.org/3.6/library/re.html), [Java](https://docs.oracle.com/javase/7/docs/api/java/util/regex/Pattern.html), [Perl](https://perldoc.perl.org/perlre.html) and other languages.

Each regular expression engine defines its own syntax, but generally the common parts of the syntaxes agree.
They are as follows.
For a more comprehensive reference see [Go's documentation on regular expression syntax](https://github.com/google/re2/wiki/Syntax).

|  RE        | Meaning                       |
|------------|-------------------------------|
| `.`        | Any character                 |
| `abc`      | a followed by b followed by c |
| `[abc]`    | a or b or c                   |
| `[^abc]`   | Any character but a or b or c |
| `a*`       | Zero or more a's (greedily)   |
| `a+`       | One or more a's               |
| `a?`       | Zero or one a                 |
| `a{5}`     | Exactly five a's              |
| `hair`     | h followed by a, etc.         |
| `\bhair\b` | Won't match chair             |


## Go regexp
The following is an example of using regular expressions from [Go's documentation for the regexp package](https://golang.org/pkg/regexp/#MatchString).
As you can see, the MatchString function takes two strings as arguments.
The first is a regular expression and the second is a string to apply the regular expression to.
The function returns a Boolean and an error object.

```go
package main

import (
	"fmt"
	"regexp"
)

func main() {
	matched, err := regexp.MatchString("foo.*", "seafood")
	fmt.Println(matched, err)
	matched, err = regexp.MatchString("bar.*", "seafood")
	fmt.Println(matched, err)
	matched, err = regexp.MatchString("a(b", "seafood")
	fmt.Println(matched, err)
}
```

The function's signature is as follows.

```go
func MatchString(pattern string, s string) (matched bool, err error)
```

### Regexp struct

More complex functionality is available through the `Regexp` struct defined in that package.
You don't create an instance of `Regexp` directly, you usually create it using the `Compile` or `MustCompile` functions.

```go
func Compile(expr string) (*Regexp, error)
func MustCompile(str string) *Regexp
```

Both functions take a single string as input, containing a regular expression.
Note that in Go you should denote regular expression strings using back-ticks, rather than double quotes, to avoid Go escaping characters in the string.

```go
var r := "\bdog\b" // Will cause a problem.
var s := `\bdog\b` // Will not cause a problem.
```

### Compiling regular expressions

The `Compile` function returns an instance of `Regexp` representing the regular expression, and an error.
The `MustCompile` function is similar, but panics instead of returning an error if the regular expression cannot be parsed.

The reason the functions use the word *compile* is because they convert the string form of the regular expression into a data structure that can be executed on a string to be matched.
Compiling a regular expression is expensive, so if you are repeatedly using the same regular expression in your program you should endeavour to compile it only once.
For those interested, Go's regexp documentation links to [a discussion of what happens behind the scenes](https://swtch.com/~rsc/regexp/regexp1.html).

### Matching

Two fundamental methods defined on the `Regexp` struct are `Match` and `MatchString`.
They are similar, except that `Match` accepts an array of bytes and `MatchString` accepts a string.

```go
func (re *Regexp) MatchString(s string) bool
func (re *Regexp) Match(b []byte) bool
```

The difference between arrays of bytes and strings are discussed [in the Go blog](https://blog.golang.org/strings).
The key thing to remember is that strings are immutable in Go, so if you want to change a string then you need to use the byte version.
You can usually avoid that situation, however, and just use the string form which is a bit more intuitive.

### FindAllStringSubmatchIndex

There are sixteen methods defined on `Regexp` to find matches to regular expressions.
All begin with `Find` and then some or all of the four words `All`, `String`, `Submatch` and `Index`.
For example, `Find()`, `FindAllString()`, `FindIndex()` and `FindAllStringSubmatchIndex()` are all such methods.
The documentation even gives the method names as a regular expression: `Find(All)?(String)?(Submatch)?(Index)?`.
These are covered in the [package documentation](https://golang.org/pkg/regexp/#pkg-overview).
Here are some samples of the method signatures.

```go
func (re *Regexp) Find(b []byte) []byte
func (re *Regexp) FindAll(b []byte, n int) [][]byte
func (re *Regexp) FindAllString(s string, n int) []string
func (re *Regexp) FindAllStringIndex(s string, n int) [][]int
```

### Replace
The package also provides several replace functions, that provide functionality akin to find and replace in programming text editors.

```go
func (*Regexp) ReplaceAllString
func (re *Regexp) ReplaceAll(src, repl []byte) []byte
```


## Regular languages
Regular expressions have a rich history in computer science.
They are closely related to [regular languages](http://www.geeksforgeeks.org/regular-expressions-regular-grammar-and-regular-languages/) and [finite automata](https://swtch.com/~rsc/regexp/regexp1.html).