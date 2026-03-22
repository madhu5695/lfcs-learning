# LFCS Day 2 Learning: Filesystem Commands

This document covers core Linux filesystem commands essential for LFCS exam preparation.

---

## 1. `ls`  
- Lists the contents of the directory  

**Syntax:**  
```bash
ls [options] <directory-path>

Options:

-l : Long listing format (detailed file info)
-a : Show all files including hidden (. files)
-h : Human-readable sizes (KB, MB, GB)
-t : Sort by modification time (newest first)
-r : Reverse sorting order
-R : List directories recursively
-d : Show directory itself, not contents
-S : Sort files by size (largest first)
-i : Display inode number of files
--color : Display files with colors by type

Combined options:

-lh : Long listing + human-readable sizes
-la : Long listing + include hidden files
-ltr : Long listing + sort by time + reverse (oldest first)
-lah : Long + hidden + human-readable

Examples:

ls -a
ls -lart /var/logs
ls --color=auto -l
2. pwd
Prints the current working directory

Syntax:

pwd [options]

Options:

-L : Shows logical path (with symlinks)
-P : Shows actual physical path

Examples:

pwd -L
pwd -P

Environment variable: $PWD

3. cd
Navigate the filesystem (must have execute permission on directories)

Syntax:

cd [OPTION] [DIRECTORY]

Options:

-L : Shows logical path (with symlinks)
-P : Shows actual physical path
-e : Error handling

Examples:

cd /home/bob
cd ../pipe
cd -P /var

Common Usage:

cd [path] → Navigate (absolute or relative)
cd or cd ~ → Go to home directory
cd .. → Move up one level
cd - → Return to previous directory
cd / → Go to root directory
cd ~username → Go to another user’s home directory
4. touch
Create empty files or update timestamps

Syntax:

touch [options] [file_name...]

Options:

-a : Change access time only
-m : Change modification time only
-c : Do not create new file
-r : Use reference file’s timestamp
-t : Set a custom timestamp

Timestamp format: [[CC]YY]MMDDhhmm[.ss]

Examples:

touch file1.txt
touch gfg1.txt gfg2.txt gfg3.txt
touch -a file.txt
touch -m file.txt
touch -c oldfile.txt
touch -r reference.txt target.txt
touch -t 202510231230.30 file.txt
5. mkdir
Create one or more directories

Syntax:

mkdir [OPTION] [DIRECTORY-NAME]

Options:

-p : Creates parent directories as needed; ignores existing directories
-m : Sets directory permissions
-v : Displays a message for each directory created
-Z : Sets SELinux security context

Examples:

mkdir myfolder
mkdir dir1 dir2 dir3
mkdir -p parent/child/grandchild
mkdir -v dir1
mkdir -m 755 mydir

Note: mv cannot move across filesystems without physically copying and deleting

6. cp
Copy files and directories

Syntax:

cp [options] <source> <destination>

Options:

-r : Recursive
-i : Prompt before overwrite
-p : Preserve file attributes (permissions, ownership, timestamps)
-a : Archive (best for full copy)
-v : Verbose output
-n : Prevent overwrite
-u : Copy if source is newer
-f : Force overwrite

Examples:

cp file1.txt file2.txt
cp file1.txt /tmp/
cp file1 file2 file3 /tmp/
cp -r dir1 dir2
cp -p file1 /tmp/
7. mv
Move or rename files/directories

Syntax:

mv [options] <source> <destination>

Options:

-i : Prompt before overwrite
-f : Force overwrite
-n : No overwrite
-u : Move if source is newer or destination missing
-v : Verbose
-b : Backup destination file before overwrite

Examples:

mv -i file1 file2
mv -f file1 file2
mv -v file1 /tmp/
8. rm
Delete files or directories

Syntax:

rm [options] <file/directory>

Options:

-i : Prompt before each deletion
-I : Prompt once before deleting >3 files or recursively
-r : Delete directories recursively
-f : Force delete, ignore nonexistent files
-v : Verbose
-d : Remove empty directories

Examples:

rm file1.txt
rm file1 file2 file3
rm -r dir1

Note: rm is permanent; files are not moved to a “Recycle Bin”.
