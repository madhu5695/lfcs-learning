# Day 1: System Documentation (LFCS Preparation)

This file contains my Day 1 LFCS preparation notes focusing on Linux **system documentation tools**.

---

## 1. `help`

A standalone command for shell built-ins.

**Syntax:**
```bash
help [options] <command>
````

**Options:**

* `-d` : Displays a brief description of the command.
* `-m` : Formats the information in a style similar to a manual (man) page.
* `-s` : Shows only a short usage synopsis or syntax.

**Examples:**

```bash
help -m cd
help -d pwd
```

**Limitations:** Doesn't work for external commands like `ls`, `mkdir`, etc.

---

## 2. `--help`

A flag appended to other commands.

**Syntax:**

```bash
<command> --help
```

**Examples:**

```bash
ls --help
mkdir --help
```

---

## 3. `man` pages

Provides detailed information about commands, configuration files, or system calls.

**Man sections:**

* **1:** General user commands (e.g., `ls`, `cat`, `rm`)
* **2:** System calls provided by the kernel (e.g., `open`, `read`, `write`)
* **3:** Library functions, primarily the C standard library
* **4:** Special files (usually devices in `/dev`)
* **5:** File formats and configuration file conventions (e.g., `/etc/passwd`)
* **6:** Games
* **7:** Miscellaneous
* **8:** System administration and privileged commands (e.g., `iptables`)

**Syntax:**

```bash
man [options] [section-number] <command>
```

**Options:**

* `-f` : Display a concise one-line description (same as `whatis`)
* `-k` : Search for commands related to a keyword
* `-a` : Display all matching manual pages
* `-w` : Return the location of the manual page
* `-I` : Make the search case-sensitive

**Examples:**

```bash
man -f journalctl
man 5 passwd
```

**Navigation:**

* `/keyword` : Search inside man page
* `n` : Next match
* `N` : Previous match
* `Spacebar` : Move forward one page
* `Enter` : Move forward one line
* `B` : Move backward one page
* `Q` : Quit the manual viewer

---

## 4. `apropos`

Search the manual page descriptions for keywords or topics.
Similar to `man -k`.

**Syntax:**

```bash
apropos [options] <keyword>
```

**Options:**

* `-e` : Exact match
* `-d` : Debug messages
* `-w` : Wildcard search
* `-a` : Logical AND of multiple keywords
* `-l` : Stop output trimming
* `-C` : Use user configuration files
* `-s` : Search specific man sections
* `-M` : Set search path
* `-m` : Look in other OS manuals
* `-L` : Set locale for search
* `-r` : Interpret keywords as regex

**Examples:**

```bash
apropos directory
apropos -a list directory
apropos "list directory"
apropos delete terminate remove
apropos set
apropos -e set
apropos -s 1,8 list
apropos '^list'
apropos "zip(note|cloak|info)"
apropos -a -s 3,8 "^list" "(implementation|devices|users)"
```

---

## 5. `info`

Structured and detailed documentation tool.

**Syntax:**

```bash
info [options] <command>
```

**Options:**

* `-a` : Use all matching manuals
* `-k` : Lookup STRING in all indices
* `-d` : Add DIR to INFOPATH
* `-f` : Specify manual to visit
* `-h` : Display help
* `-n` : Specify nodes
* `-o` : Output selected nodes to file
* `-O` : Go to command-line options node
* `-v` : Assign value to Info variable
* `-w` : Print physical location of Info file

**Examples:**

```bash
info -a cvs
info -k cvs
info -d cvs
info -O cvs
info -w cvs
```

---

## 6. `whatis`

Provides brief description of a command or system component.

**Syntax:**

```bash
whatis [options] <command>
```

**Options:**

* `-a` : Search names and descriptions
* `-n` : Specify manual section
* `-r` : Interpret keyword as regex
* `-s` : Specify sections

**Examples:**

```bash
whatis -a grep
whatis -n 5 passwd
whatis -r '^ls'
whatis du
```

---

## 7. `/usr/share/doc`

Contains package documentation, examples, and README files.

**Examples:**

```bash
ls /usr/share/doc/tar
ls /usr/share/doc/tar/examples
```

---

## Usage Strategy (LFCS)

* Use `--help` → Quick syntax
* Use `man` → Detailed options
* Use `man -k` / `apropos` → Find commands
* Use `whatis` → Quick description
* Use `/usr/share/doc` → Examples

```

---
