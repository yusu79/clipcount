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

#### CLI
- Run it as is:
```bash:
# Output the character count including whitespace characters
$ clipcount
21
```
- Remove half-width spaces and output:
```bash:
# One half-width space is removed and output
$ clipcount -s
20
```
- Remove full-width spaces:
```bash:
# The above spaces are half-width spaces, so nothing changes.
$ clipcount -S
21
```
- Remove line breaks:
```bash:
# Line breaks are removed and output
$ clipcount -b
19
```
- Remove all whitespace characters:
```bash:
# All whitespace characters are removed.
$ clipcount -sSbt
18
```
Alternatively, using `--split` yields the same result.
```bash:
# All whitespace characters are removed.
$ clipcount --split
18
```
- Calculate half-width alphanumeric characters as 0.5 and output:
```bash:
# Half-width alphanumeric characters are counted as 0.5
$ clipcount -m
10.5
```

#### Import
It can also be used in a Python file with "import".
As a note, when passing commands to options, please exclude "`-`" or "`--`".
```python:
# Store the character count of the clipboard in a variable
from clipcount import clipcount
x = clipcount({"split"})
print(x)
```
```bash:
# Produces the same result as "clipcount --split"
$ python foo.py
18
```

## Usage
clipcount is a Python package that "reads the clipboard and outputs the character count."
By default, it outputs the "character count including whitespace characters (line breaks, half-width spaces, full-width spaces, tabs)."
It also calculates both half-width and full-width characters as "the same 1 character."

With various options, you can output the character count after removing each whitespace character.
When used in the terminal, options can be combined. For example, to output "the character count after removing only full-width spaces and tabs," you can use "`clipcount -St`".

If you want to remove all whitespace characters at once, the "`--split`" option can be used. This has the same meaning as "`-bsSt`".

If you want to calculate half-width and full-width characters separately, you can use the "`-m`" option. This is an option to calculate half-width characters as "0.5 characters." It may be useful when considering character counts in situations like WordPress titles or meta keywords.

## Dependencies
- [pyperclip](https://github.com/asweigart/pyperclip)
