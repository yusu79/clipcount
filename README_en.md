# clipcount
![GitHub License](https://img.shields.io/github/license/yusu79/clipcount)
![PyPI - Version](https://img.shields.io/pypi/v/clipcount)
![PyPI - Downloads](https://img.shields.io/pypi/dm/clipcount)

This is a Python package that reads text from the clipboard and outputs the character count.

<!-- omit in toc -->
- [Setup](#setup)
- [Quick Usage](#quick-usage)
- [Usage](#usage)
- [Dependencies](#dependencies)
- [Dev-diary](#dev-diary)

## Setup
Install via pip:
```bash:
pip install clipcount
```

### Handling "Warning" on Windows
If you encounter a "`WARNING: The script clipcount.exe is installed in 'path' which is not on PATH.`" when running the above command on Windows, please refer to the following article.

Reference: [【Python Windows】pip install でPATHが通らない時の解決方法 | ゆすノート](https://yusu79.com/python-path-issue/)

If you see "`WARNING: Failed to write executable - trying to use .deleteme logic`", please refer to the following article:

Reference: [【Windows】警告が出て「pip install」できない時の解決方法 | ゆすノート](https://yusu79.com/pip-install-failure-fix/)

## Quick Usage
| Option             | Description                                                              | 
| ------------------ | ------------------------------------------------------------------------ | 
| clipcount -h       | Display the help screen.                                                 | 
| clipcount --help_jp| Display the help screen in Japanese.                                     | 
| clipcount -b       | Output the character count after removing line breaks (`\n` or `\r\n`). | 
| clipcount -s       | Output the character count after removing half-width spaces.            | 
| clipcount -S       | Output the character count after removing full-width spaces.            | 
| clipcount -t       | Output the character count after removing tab characters (`\t`).       | 
| clipcount --split  | Output the character count after removing all whitespace characters (line breaks, half-width spaces, full-width spaces, tabs). | 
| clipcount -m       | Calculate characters with half-width alphanumeric characters as 0.5 and output the character count.                    | 

Use it as a command-line tool to output character counts on the terminal:
```bash:
clipcount [options]
```

It can also be imported into Python files:
```python:
from clipcount import clipcount
x = clipcount({options})
```

### Example
*Note: The examples below are executed in a "Windows" environment, so line breaks are counted as "`\r\n`".*

Copy the following text and run clipcount:
```md:
Read
clipboard text.
```

#### CUI (Command Line Interface)

* Run as is:

```bash
# Outputs the total number of characters including all whitespace
$ clipcount
23
```

* Remove half-width spaces:

```bash
# The whitespace in this example is full-width, so nothing changes
$ clipcount -s
23
```

* Remove full-width spaces:

```bash
# One full-width space is removed from the count
$ clipcount -S
22
```

* Remove newline characters:

```bash
# Newline characters are removed from the count
$ clipcount -b
21
```

* Remove all whitespace characters:

```bash
# All whitespace characters are removed
$ clipcount -sSbt
20
```

Or use `--split` to get the same result:

```bash
# All whitespace characters are removed
$ clipcount --split
20
```

* Count half-width alphanumeric characters as 0.5:

```bash
# Half-width alphanumeric characters count as 0.5
$ clipcount -m
17.5
```

* Remove all whitespace and count half-width characters as 0.5:

```bash
$ clipcount -m --split
15.5
```



#### Import (Using clipcount in Python)

You can import the `clipcount` function:

```python
# Store the clipboard character count in a variable
from clipcount import clipcount
x = clipcount({"--split"})
print(x)
```

```bash
# Produces the same result as `clipcount --split`
$ python hoge.py
20
```

**Important note:** When using `import` in Python, do **not** combine options like `-sSbt`. Each option must be passed **individually in a set**.

```python
from clipcount import clipcount

# Correct usage (pass each option separately)
x = clipcount({"-s", "-S", "-b", "-t"})
print(x)

# Incorrect: passing combined options will not work as expected
y = clipcount({"-sSbt"})
print(y)
```

```bash
$ python hoge.py
20  # Output is as if `clipcount -sSbt` was run correctly
23  # The options do not work, so the original character count is returned
```

## Usage
`clipcount` is a Python package that counts the number of characters in the clipboard.

By default, it counts all characters, including whitespace such as newlines, half-width spaces, full-width spaces, and tabs. It does not distinguish between half-width and full-width characters; all characters are counted as **1 character** each.

You can also count characters after removing specific types of whitespace. For example, use `-b` to remove newlines, `-s` to remove half-width spaces, `-S` to remove full-width spaces, and `-t` to remove tabs.

If you want to remove all whitespace at once, you can use the `--split` option. This is equivalent to using `-b -s -S -t`.

Additionally, if you want to distinguish between half-width and full-width characters when counting, you can use the `-m` option. With this option, half-width characters are counted as **0.5**, while full-width characters are counted as 1. This can be useful when checking character counts for WordPress titles, meta keywords, or similar contexts.

You can also combine options in the terminal. For example, to remove only full-width spaces and tabs before counting, you can run:

```bash
clipcount -St
```

You can also use `clipcount` from Python by importing it. In this case, you should pass each option individually as a set, not as a combined string. For example:

```python
from clipcount import clipcount

x = clipcount({"--split"})
print(x)
```

**Important notes:**

* When using Python, do **not** combine options like `-sSbt`; pass each option separately in the set.
* All options must start with a hyphen (`-`). Passing options that do not start with a hyphen will raise an error.


## Dependencies
- [pyperclip](https://github.com/asweigart/pyperclip)

## Dev-diary
- [【個人開発】文字数を計測できる「clipcount」 | ゆすノート](https://yusu79.com/dev-clipcount/)
