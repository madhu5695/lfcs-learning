# Day 2: LFCS - Filesystem Commands

## 1. `ls`

Lists the contents of the directory.

**Syntax:**

```bash
ls [options] <directory-path>
```

**Options:**

* `-l` : Long listing format (detailed file info)
* `-a` : Show all files including hidden (`.` files)
* `-h` : Show sizes in human-readable format (KB, MB, GB)
* `-t` : Sort files by modification time (newest first)
* `-r` : Reverse the sorting order
* `-R` : List directories recursively
* `-d` : Show directory itself, not its contents
* `-S` : Sort files by size (largest first)
* `-i` : Display inode number of files
* `--color` : Display files with colors by type

**Combined options:**

* `-lh` : Long listing + human-readable sizes
* `-la` : Long listing + include hidden files
* `-ltr` : Long listing + sort by time + reverse (oldest first)
* `-lah` : Long + hidden + human-readable

**Examples:**

```bash
ls -a
ls -lart /var/logs
ls --color=auto -l
```

---

## 2. `pwd`

Prints the current working directory.

**Syntax:**

```bash
pwd [options]
```

**Options:**

* `-L` : Shows logical path (with symlinks)
* `-P` : Shows actual physical path

**Examples:**

```bash
pwd -L
pwd -P
```

**Environment variable:** `$PWD`

---

## 3. `cd`

Navigate the filesystem. Requires execute (`x`) permission on directories.

**Syntax:**

```bash
cd [OPTION] [DIRECTORY]
```

**Options:**

* `-L` : Shows logical path (with symlinks)
* `-P` : Shows actual physical path
* `-e` : Error handling

**Examples:**

```bash
cd /home/bob
cd ../pipe
cd -P /var
```

**Common Usage:**

* `cd [path]` → Navigate to absolute (`/var/log`) or relative (`Documents`) path
* `cd` or `cd ~` → Go to home directory
* `cd ..` → Move up one level
* `cd -` → Return to previous directory
* `cd /` → Go to root directory
* `cd ~username` → Go to another user’s home directory

---

## 4. `touch`

Create empty files or update file timestamps.

**Syntax:**

```bash
touch [options] [file_name...]
```

**Options:**

* `-a` : Change access time only
* `-m` : Change modification time only
* `-c` : Do not create new file
* `-r` : Use reference file’s timestamp
* `-t` : Set a custom timestamp

**Timestamp format:**

```text
[[CC]YY]MMDDhhmm[.ss]
```

* `CCYY` : Year
* `MM` : Month
* `DD` : Day
* `hhmm` : Hours and Minutes
* `.ss` : Optional Seconds

**Examples:**

```bash
touch file1.txt
touch gfg1.txt gfg2.txt gfg3.txt
touch -a file.txt
touch -m file.txt
touch -c oldfile.txt
touch -r reference.txt target.txt
touch -t 202510231230.30 file.txt
```

---

Do you want me to do that?
