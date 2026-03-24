# Day 4 Learning - LFCS

## 1. find
- Search for files and directories in a directory hierarchy based on different conditions (name, size, type, time, permissions, etc.).

**Syntax:**
```bash
find [path] [options] [expression]
````

**Options:**

* `-name` / `-iname` : Search files by name (-iname ignores case)
* `-type` : Filter by file type (file, directory, link, etc.)
* `-size` : Find files based on size (KB, MB, GB, etc.) [N -> number of the size, + -> greater than N, - -> lesser than N]
* `-mtime` : Filter files by last modification time (in days) [-n -> within n days, +n -> more than n days ago, n -> exactly n days ago]
* `-mmin` : Filter files by last modification time (in minutes) [-n -> within n minutes, +n -> more than n minutes ago, n -> exactly n minutes ago]
* `-user` : Find files owned by a specific user
* `-perm` : Search files by permission settings [- -> at least match, / -> any of these match, no prefix -> exact match]
* `-exec` / `-delete` : Perform actions on found files (run command or delete)
* `-maxdepth` : Limit how deep find searches in directories

---

### 1.1 Search by Name

| Option              | Description      |
| ------------------- | ---------------- |
| `-name "filename"`  | Case-sensitive   |
| `-iname "filename"` | Case-insensitive |

**Example:**

```bash
find /home -iname "*.txt"
```

---

### 1.2 Filter by File Type

| Type             | Code |
| ---------------- | ---- |
| Regular file     | `f`  |
| Directory        | `d`  |
| Symbolic link    | `l`  |
| Character device | `c`  |
| Block device     | `b`  |

**Example:**

```bash
find /var -type d -name "logs"
```

---

### 1.3 Size-Based Search

| Syntax | Meaning                           |
| ------ | --------------------------------- |
| `+N`   | Greater than N                    |
| `-N`   | Less than N                       |
| `N`    | Exactly N                         |
| Units  | `c`=bytes, `k`=KB, `M`=MB, `G`=GB |

**Example:**

```bash
find /var/log -type f -size +100M
```

---

### 1.4 Time-Based Search

| Option      | Description                      |
| ----------- | -------------------------------- |
| `-mtime N`  | Modified exactly N days ago      |
| `-mtime +N` | Modified more than N days ago    |
| `-mtime -N` | Modified within N days           |
| `-mmin N`   | Modified exactly N minutes ago   |
| `-mmin +N`  | Modified more than N minutes ago |
| `-mmin -N`  | Modified within N minutes        |

**Example:**

```bash
find /home -type f -mtime -7
```

---

### 1.5 Search by User

```bash
find /path -user username
```

---

### 1.6 Permission Search

| Syntax       | Meaning                        |
| ------------ | ------------------------------ |
| `-perm 644`  | Exact permissions              |
| `-perm -644` | At least these permissions set |
| `-perm /644` | Any of these bits set          |

**Example:**

```bash
find /srv -type d -perm -755
```

---

### 1.7 Depth Control

| Option        | Description                        |
| ------------- | ---------------------------------- |
| `-maxdepth N` | Maximum directory levels to search |
| `-mindepth N` | Minimum directory levels to search |

**Example:**

```bash
find /var -mindepth 2 -maxdepth 3 -type f
```

---

### 1.8 Logical Operators

| Operator      | Meaning |
| ------------- | ------- |
| `-and` / `-a` | AND     |
| `-or` / `-o`  | OR      |
| `!` / `-not`  | NOT     |

**Example:**

```bash
find /tmp -type f ! -name "*.txt"
sudo find /home -type f \( -name "*.log" -o -name "*.conf" \)
```

---

### 1.9 Execute Commands

| Option              | Description                           |
| ------------------- | ------------------------------------- |
| `-exec <cmd> {} \;` | Run command on each file              |
| `-exec <cmd> {} +`  | Run command on multiple files at once |
| `-delete`           | Delete found files (careful!)         |

**Examples:**

```bash
# Remove all .tmp files
find /tmp -type f -name "*.tmp" -exec rm -f {} \;

# Delete files owned by user 'alice'
find /tmp -type f -user alice -delete
```

---

## 2. cat

**Description:** Read, display, and combine text files.

**Syntax:**

```bash
cat [options] <filename>
```

**Options:**

* `-n` : Numbers all lines

**Examples:**

```bash
cat filename.txt
cat file1.txt file2.txt > combined.txt
cat > file.txt
cat >> file.txt
cat -n filename.txt
cat file.txt | grep "pattern"
```

---

## 3. tac

**Description:** Displays file contents in reverse line order.

**Syntax:**

```bash
tac [options] <filename>
```

**Options:**

* `-b` : Attach the separator to the beginning of lines
* `-s <separator>` : Use a custom separator

**Examples:**

```bash
tac filename.txt
tac file1.txt file2.txt > reversed.txt
tac file.txt | head -n 5
```

---

## 4. tail

**Description:** Displays the last part of a file (default: 10 lines).

**Syntax:**

```bash
tail [options] <filename>
```

**Options:**

* `-n <number>` : Last `<number>` lines
* `-f` : Follow file in real-time

**Examples:**

```bash
tail filename.txt
tail -n 5 filename.txt
tail -f /var/log/syslog
tail -n 20 -f /var/log/syslog
```

---

## 5. head

**Description:** Displays the first part of a file (default: 10 lines).

**Syntax:**

```bash
head [options] <filename>
```

**Options:**

* `-n <number>` : First `<number>` lines

**Examples:**

```bash
head filename.txt
head -n 5 filename.txt
head -n 20 file.txt
head file1.txt file2.txt
```

---

## 6. sed

**Description:** Stream editor for filtering and transforming text.

**Syntax:**

```bash
sed [options] '<command>' <filename>
```

**Options:**

* `-n` : Suppress automatic printing of lines
* `-i` : Edit file in-place
* `-e` : Multiple commands

**Examples:**

```bash
sed 's/old/new/' file.txt
sed 's/old/new/g' file.txt
sed -n '1,5p' file.txt
sed -i 's/old/new/' file.txt
sed -e 's/foo/bar/' -e 's/baz/qux/' file.txt
```

---

## 7. cut

**Description:** Extract sections from each line, like fields or characters.

**Syntax:**

```bash
cut [options] <filename>
```

**Options:**

* `-c <range>` : Characters by position
* `-f <fields>` : Fields by number
* `-d <delimiter>` : Field delimiter

**Examples:**

```bash
cut -c 1-5 file.txt
cut -f 1,3 -d ',' file.csv
cut -f 2 file.txt
```

---

## 8. uniq

**Description:** Remove or report duplicate lines (sorted file).

**Syntax:**

```bash
uniq [options] <filename>
```

**Options:**

* `-c` : Count occurrences
* `-d` : Show duplicates only
* `-u` : Show unique lines

**Examples:**

```bash
uniq file.txt
uniq -c file.txt
uniq -d file.txt
uniq -u file.txt
sort file.txt | uniq
```

---

## 9. diff

**Description:** Compare two files line by line.

**Syntax:**

```bash
diff [options] <file1> <file2>
```

**Options:**

* `-u` : Unified format
* `-c` : Context format
* `-i` : Ignore case
* `-w` : Ignore whitespace

**Examples:**

```bash
diff file1.txt file2.txt
diff -u file1.txt file2.txt
diff -i file1.txt file2.txt
diff -w file1.txt file2.txt
```
