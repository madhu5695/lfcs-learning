# Day 5 — LFCS Learning (Regular Expressions & Text Processing)

---

# Regular Expressions

## Anchors

* `^` → Start of line
* `$` → End of line
* `\b` → Word boundary
* `\B` → Non-word boundary

## Character Classes

* `.` → Any character (except newline)
* `\w` → Word char `[a-zA-Z0-9_]`
* `\d` → Digit `[0-9]` (ERE/PCRE)
* `\s` → Whitespace (space, tab…)
* `[abc]` → Any of a, b, c
* `[^abc]` → None of a, b, c
* `[a-z]` → Any lowercase letter

## POSIX Classes (BRE/ERE)

* `[:alpha:]` → Letters
* `[:digit:]` → Digits
* `[:alnum:]` → Letters + digits
* `[:space:]` → Whitespace
* `[:upper:]` → Uppercase letters
* `[:lower:]` → Lowercase letters
* `[:punct:]` → Punctuation characters

## Quantifiers

* `*` → 0 or more (greedy)
* `+` → 1 or more (greedy) — ERE
* `?` → 0 or 1 — ERE
* `{n}` → Exactly n times
* `{n,}` → n or more times
* `{n,m}` → Between n and m times
* `*?` → 0 or more (lazy) — PCRE

## Groups & Alternation

* `(abc)` → Capture group — ERE
* `\(abc\)` → Capture group — BRE
* `(?:abc)` → Non-capturing group
* `a|b` → Alternation: a or b — ERE
* `\1` → Backreference to group 1

## BRE vs ERE

* `grep` → BRE by default
* `grep -E` → Extended RE (ERE)
* `grep -P` → Perl-compatible (PCRE)
* `egrep` → Alias for `grep -E`
* `sed` → BRE; use `-E` for ERE
* `awk` → ERE always

---

# 1. `grep` — Search Text Using Patterns

## What it does

`grep` searches for lines in files that match a **pattern (regular expression)**.

## Syntax

```bash
grep [options] PATTERN [FILE...]
```

## Common Options

* `-i` → Ignore case
* `-v` → Invert match (show non-matching lines)
* `-n` → Show line numbers
* `-r` → Recursive search in directories
* `-l` → Show only file names
* `-w` → Match whole words
* `-E` → Use extended regex (same as `egrep`)

## Regular Expression Examples

* `.` → any character
* `^` → start of line
* `$` → end of line
* `*` → zero or more
* `[abc]` → match a, b, or c
* `[^abc]` → not a, b, or c

## Usage Examples

```bash
grep "hello" file.txt
```

Finds lines containing "hello"

```bash
grep -i "^hello" file.txt
```

Case-insensitive lines starting with "hello"

```bash
grep -E "cat|dog" file.txt
```

Matches "cat" or "dog"

```bash
grep -v "error" logfile.txt
```

Shows lines NOT containing "error"

---

# 2. `sed` — Stream Editor (Edit Text)

## What it does

`sed` processes and transforms text line-by-line (replace, delete, insert, etc.).

## Syntax

```bash
sed [options] 'command' file
```

## Common Options

* `-n` → Suppress automatic printing
* `-e` → Execute multiple commands
* `-i` → Edit file in-place
* `-f` → Read commands from file

## Common Commands

* `s/old/new/` → Substitute
* `d` → Delete line
* `p` → Print line
* `a\` → Append
* `i\` → Insert

## Regex in `sed`

Same basic regex as `grep`, plus:

* `\(` `\)` → Grouping
* `\1`, `\2` → Backreferences

## Usage Examples

```bash
sed 's/foo/bar/' file.txt
```

Replace first "foo" with "bar"

```bash
sed 's/foo/bar/g' file.txt
```

Replace ALL occurrences

```bash
sed '/error/d' file.txt
```

Delete lines containing "error"

```bash
sed -n '/hello/p' file.txt
```

Print only lines with "hello"

```bash
sed 's/\(cat\)/[\1]/g' file.txt
```

Wrap "cat" with brackets

---

# 3. `awk` — Pattern Scanning & Processing

## What it does

`awk` is a powerful programming tool for **text processing and reporting**, especially column-based data.

## Syntax

```bash
awk 'pattern { action }' file
```

## Built-in Variables

* `$0` → Entire line
* `$1, $2, ...` → Columns (fields)
* `NF` → Number of fields
* `NR` → Line number

## Common Options

* `-F` → Field separator
* `-v` → Define variable

## Regex Usage

Patterns can be regular expressions:

```bash
awk '/pattern/ { action }'
```

## Usage Examples

```bash
awk '{print $1}' file.txt
```

Print first column

```bash
awk -F "," '{print $2}' file.csv
```

Print second column (CSV)

```bash
awk '/error/ {print $0}' logfile.txt
```

Print lines containing "error"

```bash
awk '$3 > 100 {print $1, $3}' file.txt
```

Print rows where column 3 > 100

```bash
awk '/^A/ {print}' file.txt
```

Lines starting with "A"

---

# Key Differences

| Feature       | `grep` | `sed`          | `awk`            |
| ------------- | ------ | -------------- | ---------------- |
| Purpose       | Search | Edit/Transform | Process/Analyze  |
| Works on      | Lines  | Lines          | Fields (columns) |
| Regex support | Yes    | Yes            | Yes              |
| Complexity    | Simple | Medium         | Powerful         |

---

# When to Use What

* Use **`grep`** → When you just want to **find text**
* Use **`sed`** → When you want to **modify text**
* Use **`awk`** → When you need **logic, calculations, or column processing**

---

# Quick Combined Example

```bash
grep "error" logfile.txt | sed 's/error/ERR/g' | awk '{print $1}'
```

Find "error" → replace with "ERR" → print first column

---
