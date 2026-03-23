````markdown
# LFCS Day 3: File Links, Permissions, and Special Bits

## 1. Hard Links

- A hard link is another name (directory entry) for the same file data on disk.
- In Linux, files are stored as inodes (data structures containing metadata and pointers to data blocks).
- A hard link points to the same inode as the original file.

**Inode:**
- Stores file metadata (permissions, size, owner, timestamps)
- Identified using inode number

**Commands:**
```bash
ls -i
# Shows inode number

stat <filename>
# Displays detailed file information
````

**Syntax:**

```bash
ln [target-file] [link-file]
```

**Example:**

```bash
echo "Hello" > file1.txt
ln file1.txt file2.txt
```

**Limitations:**

* Works only within the same filesystem
* Cannot be created for directories (in most cases)
* Cannot link to non-existent files

**Permissions:**

* Write + execute permission on the target directory (where the new link will be created).

**Additional Points:**

* Link count determines deletion (deleted only when count = 0)
* No original file exists after linking
* Changes in one link reflect in all
* Does not consume additional disk space
* All links share same metadata (permissions, owner, size)
* No visual difference from normal files

---

## 2. Soft Links (Symbolic Links)

* Similar to a Windows shortcut
* A soft link is a file that stores the path to another file or directory
* It acts as a pointer/reference to the target file

**Syntax:**

```bash
ln -s [target-file] [link-file]
```

**Options:**

* `-f` : Force overwrite (delete destination)
* `-n` : Don’t follow existing symlink

**Example:**

```bash
ln -s /home/abc/mypictures.jpg pic.jpg
# lrwxrwxrwx 1 user user 20 Mar 23 pic.jpg -> /home/abc/mypictures.jpg
```

**Features:**

* Has its own inode (different from target)
* Displays as `->` in `ls -l` output
* Can link files and directories
* Can cross different filesystems
* Can be created even if target does not exist

**Limitations:**

* Becomes broken (dangling link) if target is deleted
* Slight disk space is used to store path

**Permissions:**

* Permissions of soft link are usually ignored
* Access depends on target file permissions

---

## 3. `readlink`

* Displays the target (actual path) of a symbolic (soft) link.

**Syntax:**

```bash
readlink [OPTION] [symlink_name]
```

**Options:**

* `-f` : Resolves all symlinks and relative paths to give the absolute path, even if parts don’t exist.
* `-e` : Like `-f` but fails if the target does not exist.
* `-m` : Resolves paths to absolute even if some parts don’t exist, most flexible.

**Examples:**

```bash
readlink -f mylink
readlink -e mylink
readlink -m mylink
```

---

## 4. `unlink`

* Remove the symlink.
* Deletes only one link at a time.
* Use `rm` command for deleting multiple links.

**Syntax:**

```bash
unlink [symlink_name]
```

**Example:**

```bash
unlink mylink
```

**Alternate command:**

```bash
rm mylink
```

---

## 5. `chgrp`

* Change the group ownership of a file or directory.
* Only file owner or root user can change the file permission.
* User must belong to the target group (unless root).

**Syntax:**

```bash
chgrp [options] <group_name> <file_name>
```

**Options:**

* `-R` : Change group for directory and all its contents recursively
* `-v` : Show which files were changed
* `--reference=ref_file` : Change group to the same as ref_file
* `-h` : Change the symlink itself (not the target)

**Examples:**

```bash
chgrp developers project.txt
chgrp -R developers /project
chgrp --reference=template.txt project.txt
```

---

## 6. `chown`

* Change the owner and/or group of a file or directory.
* Only root can change the owner. Users can change the group if they belong to the group.
* Can change both owner and group at the same time.

**Syntax:**

```bash
chown [options] [owner][:group] <file_name>
```

**Options:**

* `-R` : Change owner/group recursively for directories and all contents
* `-v` : Show which files were changed
* `--reference=ref_file` : Set owner/group same as a reference file
* `-h` : Change symlink itself (not the target)

**Examples:**

```bash
chown alice project.txt
chown alice:developers project.txt
chown -R alice:developers /project
```

---

## 7. `chmod`

* Change the permissions (read, write, execute) of a file or directory.
* Only the file owner or root can change permissions.

**Permission Basics**

Every file has 3 sets of permissions:

| Class          | Who it affects        | Symbols                          |
| -------------- | --------------------- | -------------------------------- |
| **User (u)**   | File owner            | r (read), w (write), x (execute) |
| **Group (g)**  | Users in file’s group | r, w, x                          |
| **Others (o)** | Everyone else         | r, w, x                          |

`ls -l` shows permissions like:

```
-rwxr-xr-- 1 alice developers 1024 Mar 23 2026 project.txt
```

* `rwx` → user
* `r-x` → group
* `r--` → others

**Syntax:**

```bash
chmod [options] <mode> <file_name/directory_name>
```

* `mode` can be symbolic or numeric (octal)

**Symbolic mode:**

| Symbol | Meaning              |
| ------ | -------------------- |
| `+`    | Add permission       |
| `-`    | Remove permission    |
| `=`    | Set exact permission |

**Numeric mode:**

| Numeric | Meaning            |
| ------- | ------------------ |
| 4       | Read permission    |
| 2       | Write permission   |
| 1       | Execute permission |

**Options:**

* `-R` : Apply permissions recursively
* `-v` : Verbose → show which files were changed
* `--reference=ref_file` : Set permissions same as reference file

**Examples:**

```bash
chmod u+x project.txt
chmod g-w project.txt
chmod o=r project.txt
chmod a+r project.txt
chmod 754 project.txt
chmod -R 755 /project
chmod -v u+rw project.txt
chmod --reference=template.txt project.txt
```

---

## 8. SUID, SGID & Sticky Bit

### SUID

* SUID (Set User ID) allows executable to run with the privileges of the file owner.
* Commonly used for programs needing temporary elevated privileges.

**Syntax:**

```bash
chmod u+s <filename>
```

**Example:**

```bash
chmod u+s myfile
```

```
-rwsr-xr-x 1 root root 12345 Mar 23 2026 myprog  # s → SUID active
-r-Sr-xr-x 1 root root 12345 Mar 23 2026 myprog  # S → SUID set but owner cannot execute
```

---

### SGID

* SGID (Set Group ID) behaves differently on files vs directories:

| Type          | Effect                                                                                    |
| ------------- | ----------------------------------------------------------------------------------------- |
| **File**      | Executable runs with the **group privileges of the file**, not the user’s group           |
| **Directory** | Files created inside inherit the directory’s group, not the creating user’s primary group |

**Syntax:**

```bash
chmod g+s <filename_or_directory>
```

**Example:**

```bash
chmod g+s myprog
```

```
-rwxr-sr-x 1 alice developers 1024 Mar 23 2026 myprog # s → SGID active
-rwxr-Sr-x 1 alice developers 1024 Mar 23 2026 myprog # S → SGID set but group cannot execute
drwxrwsr-x 2 alice developers 4096 Mar 23 2026 shared # s in group execute → new files inherit group
```

---

### Sticky Bit

* Sticky Bit restricts file deletion in directories.

**Syntax:**

```bash
chmod +t <directory_name>
```

**Example:**

```bash
chmod +t stickydir/
```

```
drwxrwxrwt 5 root root 4096 Mar 23 2026 /tmp # t → Sticky Bit set, others can execute
drwxrwxrwT 5 root root 4096 Mar 23 2026 /tmp # T → Sticky Bit set, others cannot execute
```

---

### Setting Special Permissions with `chmod`

**Syntax:**

```bash
chmod [special][owner/group/others permissions] <filename/directoryname>
```

**Examples:**

```bash
chmod 4646 myfile.txt
chmod 2775 /shared
chmod 1755 /tmp
```

**Octal Representation of Special Permissions:**

| Special Bit | Value (Octal) | Meaning                                 |
| ----------- | ------------- | --------------------------------------- |
| SUID        | 4             | Set User ID                             |
| SGID        | 2             | Set Group ID                            |
| Sticky Bit  | 1             | Sticky Bit (used mostly on directories) |

```

---
