# Linux Command Reference & Study Guide

## Network+ Foundations and Introductory Linux Administration

> A practical reference for Linux command-line basics, file management,
> text processing, system administration, networking, users, groups, and
> permissions.

**Target audience:** Beginner to early-intermediate Linux learners\
**Shell examples:** Mostly Bash on common Linux distributions\
**Important:** Some commands and options differ between distributions.
Commands that change network settings, users, permissions, or system
files may require `sudo` privileges.

------------------------------------------------------------------------

# Table of Contents

1.  [Getting Started and Shell
    Basics](#1-getting-started-and-shell-basics)
2.  [Navigating the File System](#2-navigating-the-file-system)
3.  [Creating, Viewing, Copying, Moving, and Deleting
    Files](#3-creating-viewing-copying-moving-and-deleting-files)
4.  [Getting Help](#4-getting-help)
5.  [Variables, Output, and Shell
    Scripting](#5-variables-output-and-shell-scripting)
6.  [Searching and Processing Text](#6-searching-and-processing-text)
7.  [Pipes and Redirection](#7-pipes-and-redirection)
8.  [Archiving and Compression](#8-archiving-and-compression)
9.  [Important Linux Directories and
    Files](#9-important-linux-directories-and-files)
10. [Processes and System
    Information](#10-processes-and-system-information)
11. [Linux Networking](#11-linux-networking)
12. [Users and Groups](#12-users-and-groups)
13. [Linux Permissions and
    Ownership](#13-linux-permissions-and-ownership)
14. [Symbolic Links](#14-symbolic-links)
15. [Additional Useful Commands](#15-additional-useful-commands)
16. [Practical Exercises](#16-practical-exercises)
17. [Safety Notes and Common
    Mistakes](#17-safety-notes-and-common-mistakes)

------------------------------------------------------------------------

# 1. Getting Started and Shell Basics

## 1.1 What Is the Linux Terminal?

The **terminal** is a text-based interface through which you interact
with a Linux system. You type commands, the shell interprets them, and
the operating system performs the requested actions.

A **shell** is the program that reads and executes commands. Bash
(Bourne Again Shell) is one of the most common Linux shells.

A command usually has this structure:

``` bash
command [options] [arguments]
```

For example:

``` bash
ls -l /home
```

-   `ls` is the command.
-   `-l` is an option that requests a detailed listing.
-   `/home` is the argument that tells `ls` which directory to list.

Linux commands are generally case-sensitive. For example, `ls` and `LS`
are not the same command.

## 1.2 `clear` --- Clear the Terminal Screen

`clear` clears the visible terminal screen. It does not delete files,
command history, or output stored elsewhere.

``` bash
clear
```

**Useful alternative:** Press `Ctrl + L` in many terminal emulators to
clear the visible screen.

## 1.3 `exit` --- Leave a Shell Session

`exit` terminates the current shell. If you opened a nested shell, it
returns you to the previous shell. If it is your only shell, the
terminal session may close.

``` bash
exit
```

You can also press `Ctrl + D` at an empty shell prompt to send an
end-of-file signal, which commonly exits the shell.

## 1.4 `reboot` --- Restart the System

`reboot` restarts the computer. It normally requires administrator
privileges.

``` bash
sudo reboot
```

Related commands:

``` bash
sudo shutdown -r now       # Restart immediately
sudo shutdown -r +5        # Restart in five minutes
sudo shutdown -c           # Cancel a scheduled shutdown
sudo poweroff              # Power off the system
```

**Caution:** Save your work first. Restarting can terminate running
programs and interrupt file operations.

------------------------------------------------------------------------

# 2. Navigating the File System

## 2.1 `pwd` --- Print Working Directory

Although it was not in the original list, `pwd` is essential when
learning Linux navigation.

It displays the absolute path of your current directory.

``` bash
pwd
```

Example output:

``` text
/home/sina/Documents
```

This means your current working directory is `Documents`, inside `sina`,
inside `/home`.

## 2.2 `cd` --- Change Directory

`cd` changes your current working directory.

``` bash
cd /etc
```

Common uses:

``` bash
cd /home/sina       # Go to a specific directory
cd ..               # Move up one directory
cd ../..            # Move up two directories
cd ~                # Go to your home directory
cd                  # Also goes to your home directory in Bash
cd -                # Return to the previous directory
cd /                # Go to the root directory
```

### Absolute and Relative Paths

A **path** describes the location of a file or directory.

**Absolute path:** Starts from the root directory `/` and describes the
complete location.

``` bash
cd /home/sina/Documents
```

This path identifies the same location regardless of your current
directory.

**Relative path:** Describes a location in relation to your current
working directory.

Suppose your current directory is:

``` text
/home/sina
```

Then:

``` bash
cd Documents
```

takes you to:

``` text
/home/sina/Documents
```

Special path symbols:

  Symbol   Meaning
  -------- --------------------------------------
  `/`      Root directory
  `.`      Current directory
  `..`     Parent directory
  `~`      Current user's home directory
  `-`      Previous working directory (in Bash)

## 2.3 `ls` --- List Directory Contents

`ls` displays files and directories.

``` bash
ls
```

Important options:

  Option   Description
  -------- ----------------------------------------------------
  `-l`     Long format: permissions, owner, group, size, date
  `-a`     Include hidden files (names beginning with `.`)
  `-h`     Human-readable sizes; commonly used with `-l`
  `-R`     List directories recursively
  `-t`     Sort by modification time, newest first
  `-r`     Reverse the sort order
  `-S`     Sort by file size
  `-d`     List a directory itself rather than its contents

Examples:

``` bash
ls -l
ls -la
ls -lh /var/log
ls -lt
ls -lS
ls -R /home/sina
```

A useful combination:

``` bash
ls -lah
```

It shows hidden files in long format with human-readable file sizes.

### Understanding `ls -l` Output

Example:

``` text
-rw-r--r-- 1 sina students 1200 Nov 12 10:30 notes.txt
```

The fields are:

1.  `-rw-r--r--`: file type and permissions.
2.  `1`: hard-link count.
3.  `sina`: owner.
4.  `students`: group.
5.  `1200`: size in bytes.
6.  `Nov 12 10:30`: modification date and time.
7.  `notes.txt`: file name.

The first character indicates the file type. For example, `-` means
regular file and `d` means directory.

------------------------------------------------------------------------

# 3. Creating, Viewing, Copying, Moving, and Deleting Files

## 3.1 `touch` --- Create an Empty File or Update Timestamps

`touch` creates an empty file if it does not exist. If the file already
exists, it updates its access and modification timestamps by default.

``` bash
touch notes.txt
```

Create several files:

``` bash
touch file1.txt file2.txt file3.txt
```

Create a file in another directory:

``` bash
touch /home/sina/Documents/todo.txt
```

**Important:** `touch` does not normally erase the content of an
existing file.

## 3.2 `mkdir` --- Create Directories

`mkdir` creates one or more directories.

``` bash
mkdir projects
```

Important options:

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `-p`                                Create parent directories when
                                      needed; do not complain if they
                                      already exist

  `-v`                                Display a message for each
                                      directory created

  `-m MODE`                           Set permissions when creating the
                                      directory
  -----------------------------------------------------------------------

Examples:

``` bash
mkdir photos backups
mkdir -p projects/linux/day1
mkdir -v reports
mkdir -m 750 private
```

## 3.3 `nano` --- Edit Text Files

`nano` is a simple terminal-based text editor.

``` bash
nano notes.txt
```

If the file does not exist, Nano creates it when you save.

Common Nano shortcuts:

  Shortcut     Action
  ------------ -----------------------------------
  `Ctrl + O`   Write (save) the file
  `Enter`      Confirm the file name when saving
  `Ctrl + X`   Exit Nano
  `Ctrl + W`   Search inside the file
  `Ctrl + K`   Cut the current line
  `Ctrl + U`   Paste the cut text
  `Ctrl + G`   Open Nano's help

A common workflow:

1.  Run `nano notes.txt`.
2.  Type your content.
3.  Press `Ctrl + O` and then `Enter` to save.
4.  Press `Ctrl + X` to exit.

## 3.4 `cat` --- Display and Combine File Contents

`cat` reads files and writes their contents to standard output, usually
the terminal.

``` bash
cat notes.txt
```

Useful options:

  Option   Description
  -------- ------------------------------
  `-n`     Number all output lines
  `-b`     Number non-empty lines
  `-s`     Squeeze repeated blank lines

Examples:

``` bash
cat -n notes.txt
cat file1.txt file2.txt
```

Combine two files into a third:

``` bash
cat file1.txt file2.txt > combined.txt
```

**Caution:** `cat` is useful for short files. For long files, use
`less`.

## 3.5 `less` --- Read Long Files Page by Page

`less` displays text one screen at a time and allows you to move forward
and backward.

``` bash
less /var/log/syslog
```

Common keyboard controls:

  Key             Action
  --------------- -----------------------
  `Space`         Next page
  `b`             Previous page
  `Down` / `Up`   Move one line
  `/word`         Search forward
  `?word`         Search backward
  `n`             Next search match
  `N`             Previous search match
  `g`             Go to beginning
  `G`             Go to end
  `q`             Quit

Unlike `cat`, `less` does not need to print an entire large file at
once.

## 3.6 `cp` --- Copy Files and Directories

`cp` copies files or directories.

Basic syntax:

``` bash
cp SOURCE DESTINATION
```

Examples:

``` bash
cp notes.txt notes-backup.txt
cp notes.txt /home/sina/Documents/
cp file1.txt file2.txt backup/
```

Important options:

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `-r` or `-R`                        Copy directories recursively

  `-i`                                Ask before overwriting a
                                      destination

  `-v`                                Show what is being copied

  `-n`                                Do not overwrite existing files

  `-p`                                Preserve mode, ownership where
                                      permitted, and timestamps

  `-a`                                Archive mode; recursively copy
                                      while preserving many attributes
                                      and symbolic links
  -----------------------------------------------------------------------

Examples:

``` bash
cp -r projects/ projects-backup/
cp -iv notes.txt backup/
cp -a website/ website-backup/
```

**Tip:** Use `cp -i` when you want protection against accidental
overwrites.

## 3.7 `mv` --- Move or Rename Files and Directories

`mv` moves files or directories. It is also commonly used to rename
them.

Move a file:

``` bash
mv notes.txt Documents/
```

Rename a file:

``` bash
mv old-name.txt new-name.txt
```

Move and rename:

``` bash
mv report.txt Documents/final-report.txt
```

Useful options:

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `-i`                                Ask before overwriting

  `-n`                                Do not overwrite an existing
                                      destination

  `-v`                                Show the operation

  `-u`                                Move only when the source is newer
                                      or the destination is missing
  -----------------------------------------------------------------------

Examples:

``` bash
mv -i report.txt backup/
mv -v old-folder new-folder
```

## 3.8 `rename` --- Rename Multiple Files

Some Linux distributions provide a `rename` utility for batch renaming.
There are different implementations, and their syntax is not identical.

A common Perl-based version supports substitutions:

``` bash
rename 's/.txt$/.bak/' *.txt
```

This changes the `.txt` suffix to `.bak` for matching files in the
current directory.

Another implementation may use a different syntax. Check the installed
version first:

``` bash
rename --help
man rename
```

For a single file, `mv` is usually simpler and more portable.

## 3.9 `rm` --- Remove Files

`rm` deletes files. It does not normally move them to a recycle bin.

``` bash
rm notes.txt
```

Important options:

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `-i`                                Ask before each removal

  `-I`                                Ask once before removing multiple
                                      files or recursively removing

  `-f`                                Force removal; ignore missing files
                                      and do not prompt

  `-r` or `-R`                        Remove directories and their
                                      contents recursively

  `-v`                                Display what is being removed
  -----------------------------------------------------------------------

Examples:

``` bash
rm old.txt
rm -i important.txt
rm -v *.tmp
```

## 3.10 `rmdir` --- Remove Empty Directories

`rmdir` removes directories only when they are empty.

``` bash
rmdir old-folder
```

Remove empty nested directories:

``` bash
rmdir -p projects/old/empty
```

If a directory contains files, `rmdir` fails. Use `rm -r` only when you
intentionally want to remove the directory and its contents.

## 3.11 `rm -rf` --- Forcefully Remove Directories Recursively

This is a combination of two options:

-   `-r`: recursively remove directories and their contents.
-   `-f`: force removal without prompting and ignore missing files.

Example:

``` bash
rm -rf old-project/
```

This removes the directory and everything inside it.

**Extremely important safety warning:** `rm -rf` can permanently delete
large amounts of data. Always verify the path before running it. Avoid
experimenting with it as root. Never copy and execute an unfamiliar
`rm -rf` command without understanding the target.

Safer approach:

``` bash
ls -la old-project/
rm -ri old-project/
```

The interactive option asks before removing items.

------------------------------------------------------------------------

# 4. Getting Help

Linux provides several ways to learn how a command works without
searching the internet.

## 4.1 `command --help` --- Short Command Help

Many commands support `--help` to display a short usage guide.

``` bash
ls --help
cp --help
ip --help
```

Some older or non-GNU commands use `-h` or have different help behavior.
`--help` is common, but not universal.

## 4.2 `man` --- Manual Pages

`man` opens the manual page for a command.

``` bash
man ls
man chmod
man 5 passwd
```

Manual pages are organized into sections. Important sections include:

  Section   Contents
  --------- --------------------------------------
  1         User commands
  2         System calls
  3         Library functions
  5         File formats and configuration files
  8         System administration commands

For example:

``` bash
man 5 passwd
```

opens the manual page describing the `/etc/passwd` file format, rather
than the user-management command.

Useful keys inside `man`:

-   `Space`: next page
-   `b`: previous page
-   `/word`: search
-   `n`: next match
-   `q`: quit

## 4.3 `info` --- Detailed Documentation

`info` displays documentation in the GNU Info format.

``` bash
info coreutils
info ls
```

It may contain more structured detail than a short help message.

## 4.4 `whatis` --- One-Line Command Description

`whatis` displays a short description of a command based on the
manual-page database.

``` bash
whatis ls
whatis chmod
```

If it cannot find a description, the manual database may need updating
or the relevant manual page may not be installed.

## 4.5 `whereis` --- Locate a Command's Binary, Source, and Manual

`whereis` searches standard locations for a command's binary, source
files, and manual pages.

``` bash
whereis ls
whereis bash
```

Example output might include:

``` text
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
```

This indicates the executable and a manual-page file.

**Related command:** `which` commonly shows the executable that would be
selected from your current `PATH`.

``` bash
which python3
```

For shell built-ins and aliases, `type` is often more informative:

``` bash
type cd
type ls
```

------------------------------------------------------------------------

# 5. Variables, Output, and Shell Scripting

## 5.1 What Is a Shell Variable?

A **variable** is a named place where the shell stores a value.
Variables can hold text, numbers, paths, and other data.

Create a variable:

``` bash
name="Sina"
```

Read its value by adding `$`:

``` bash
echo "$name"
```

Output:

``` text
Sina
```

Important rules:

-   Do not put spaces around `=` when assigning a shell variable.
-   Use `$variable` to read its value.
-   Quoting variables with `"$variable"` is usually safer because it
    preserves spaces and prevents unwanted word splitting and wildcard
    expansion.

Incorrect:

``` bash
name = "Sina"
```

Correct:

``` bash
name="Sina"
```

## 5.2 Local Shell Variables and Environment Variables

A normal shell variable is available in the current shell.

``` bash
course="Network+"
```

An **environment variable** is passed to child processes.

``` bash
export course="Network+"
```

View environment variables:

``` bash
printenv
printenv PATH
env
```

The `PATH` variable contains directories where the shell looks for
executable commands.

``` bash
echo "$PATH"
```

## 5.3 `echo` --- Display Text and Variable Values

`echo` prints text to standard output.

``` bash
echo "Hello Linux"
```

Print a variable:

``` bash
username="Sina"
echo "Welcome, $username"
```

Common option:

``` bash
echo -n "Enter your name: "
```

`-n` prevents the usual trailing newline in many implementations.

For more predictable formatting in scripts, `printf` is often preferred:

``` bash
printf 'User: %s\n' "$username"
```

## 5.4 Basic Shell Scripting

A **shell script** is a text file containing shell commands that can be
executed as a program.

Create a script:

``` bash
nano hello.sh
```

Write:

``` bash
#!/usr/bin/env bash

echo "Hello, Linux!"
echo "Today I am learning shell scripting."
```

The first line is called the **shebang**. It tells the operating system
which interpreter should run the script.

Run it using Bash:

``` bash
bash hello.sh
```

Or make it executable:

``` bash
chmod +x hello.sh
./hello.sh
```

### Variables in a Script

``` bash
#!/usr/bin/env bash

name="Sina"
echo "Hello, $name"
```

### Reading User Input

``` bash
#!/usr/bin/env bash

read -r -p "Enter your name: " name
echo "Welcome, $name"
```

### Conditional Statements

``` bash
#!/usr/bin/env bash

read -r -p "Enter a number: " number

if [ "$number" -gt 10 ]; then
    echo "The number is greater than 10."
else
    echo "The number is 10 or less."
fi
```

Common numeric comparison operators:

  Operator   Meaning
  ---------- -----------------------
  `-eq`      Equal
  `-ne`      Not equal
  `-gt`      Greater than
  `-ge`      Greater than or equal
  `-lt`      Less than
  `-le`      Less than or equal

Common file tests:

  Test        Meaning
  ----------- ---------------------------------
  `-e FILE`   File or path exists
  `-f FILE`   Regular file exists
  `-d DIR`    Directory exists
  `-r FILE`   Read permission is available
  `-w FILE`   Write permission is available
  `-x FILE`   Execute permission is available

Example:

``` bash
if [ -f "/etc/passwd" ]; then
    echo "The passwd file exists."
fi
```

### Loops

A `for` loop:

``` bash
for item in one two three; do
    echo "$item"
done
```

A `while` loop:

``` bash
count=1

while [ "$count" -le 3 ]; do
    echo "Count: $count"
    count=$((count + 1))
done
```

### Exit Status

Every command returns an exit status. Conventionally:

-   `0` means success.
-   A non-zero value means some kind of failure or special condition.

View the previous command's exit status:

``` bash
echo $?
```

Example:

``` bash
ls /etc
echo $?
```

### Useful Script Safety Practices

-   Quote variable expansions: `"$file"`.
-   Use meaningful variable names.
-   Check whether commands succeed when later steps depend on them.
-   Avoid running scripts as root unless necessary.
-   Test scripts on sample files before using them on important data.

------------------------------------------------------------------------

# 6. Searching and Processing Text

These commands are especially useful when examining configuration files,
logs, command output, and security events.

## 6.1 `head` --- Display the Beginning of a File

`head` displays the first 10 lines by default.

``` bash
head notes.txt
```

Important options:

  Option        Description
  ------------- --------------------------------
  `-n NUMBER`   Display the first NUMBER lines
  `-c NUMBER`   Display the first NUMBER bytes

Examples:

``` bash
head -n 5 notes.txt
head -c 100 notes.txt
```

## 6.2 `tail` --- Display the End of a File

`tail` displays the last 10 lines by default.

``` bash
tail notes.txt
```

Important options:

  Option        Description
  ------------- ---------------------------------------------------------------
  `-n NUMBER`   Display the last NUMBER lines
  `-f`          Keep watching the file as new lines are added
  `-F`          Follow a file by name and retry if it is rotated or recreated
  `-c NUMBER`   Display the last NUMBER bytes

Examples:

``` bash
tail -n 20 /var/log/syslog
tail -f /var/log/auth.log
```

`tail -f` is particularly useful for monitoring logs in real time.

The exact log file names differ between distributions. Some systems use
`journalctl` instead of traditional text log files.

## 6.3 `grep` --- Search for Text Patterns

`grep` searches input for lines matching a pattern.

``` bash
grep "error" application.log
```

Important options:

  Option         Description
  -------------- -----------------------------------------------------------
  `-i`           Ignore uppercase/lowercase differences
  `-n`           Show line numbers
  `-v`           Show lines that do not match
  `-r` or `-R`   Search directories recursively
  `-l`           Show only names of files with matches
  `-c`           Count matching lines in each file
  `-w`           Match whole words
  `-E`           Use extended regular expressions
  `-F`           Treat the pattern as fixed text, not a regular expression
  `-A N`         Show N lines after each match
  `-B N`         Show N lines before each match
  `-C N`         Show N lines before and after each match

Examples:

``` bash
grep "root" /etc/passwd
grep -i "failed" auth.log
grep -n "error" application.log
grep -v "^#" config.conf
grep -r "Listen" /etc/
grep -E "error|warning" application.log
```

Search for a word in a file:

``` bash
grep -w "admin" users.txt
```

Search recursively while displaying line numbers:

``` bash
grep -rn "password" /etc/
```

**Security note:** Searching for sensitive terms may reveal secrets. Do
not copy or share credentials found in configuration files or logs.

## 6.4 `find` --- Search for Files and Directories

`find` searches a directory tree using criteria such as name, type,
size, owner, and modification time.

Basic syntax:

``` bash
find PATH CONDITIONS
```

Examples:

Find a file by name:

``` bash
find /home/sina -name "notes.txt"
```

Ignore case:

``` bash
find /home/sina -iname "notes.txt"
```

Find directories:

``` bash
find /var -type d
```

Find regular files:

``` bash
find /var/log -type f
```

Find files ending in `.log`:

``` bash
find /var/log -type f -name "*.log"
```

Find files larger than 100 MB:

``` bash
find /home -type f -size +100M
```

Find files modified within the last day:

``` bash
find /home/sina -type f -mtime -1
```

Important options and tests:

  Test               Description
  ------------------ ---------------------------------
  `-name PATTERN`    Match a case-sensitive name
  `-iname PATTERN`   Match a case-insensitive name
  `-type f`          Regular files
  `-type d`          Directories
  `-size +100M`      Larger than 100 MB
  `-mtime -7`        Modified less than 7 days ago
  `-user NAME`       Owned by a user
  `-perm MODE`       Match permission criteria
  `-maxdepth N`      Limit directory traversal depth

Run an action on matches:

``` bash
find . -type f -name "*.tmp" -print
```

To delete matching files, first inspect the results. Only when you are
certain:

``` bash
find . -type f -name "*.tmp" -delete
```

Avoid combining broad searches with destructive actions until you have
verified exactly what will match.

## 6.5 `sort` --- Sort Lines of Text

`sort` sorts lines alphabetically by default.

``` bash
sort names.txt
```

Important options:

  Option   Description
  -------- ---------------------------------------------
  `-r`     Reverse the order
  `-n`     Sort numerically
  `-u`     Remove duplicate lines in the sorted output
  `-k`     Sort by a specified field/key
  `-t`     Specify a field separator
  `-f`     Ignore case differences

Examples:

``` bash
sort names.txt
sort -r names.txt
sort -n numbers.txt
sort -u users.txt
```

Sort a colon-separated file by the third field:

``` bash
sort -t: -k3,3n /etc/passwd
```

## 6.6 `cut` --- Extract Parts of Each Line

`cut` extracts selected characters, bytes, or delimiter-separated
fields.

Important options:

  Option           Description
  ---------------- -----------------------------------------------
  `-d`             Set the field delimiter
  `-f`             Select fields
  `-c`             Select character positions
  `--complement`   Select everything except the specified fields

Examples:

Extract the first field from a colon-separated file:

``` bash
cut -d: -f1 /etc/passwd
```

Extract the first and third fields:

``` bash
cut -d: -f1,3 /etc/passwd
```

Extract characters 1 through 5:

``` bash
cut -c1-5 names.txt
```

## 6.7 `wc` --- Count Lines, Words, and Bytes

`wc` reports counts for text input.

``` bash
wc notes.txt
```

Options:

  Option   Description
  -------- ------------------
  `-l`     Count lines
  `-w`     Count words
  `-c`     Count bytes
  `-m`     Count characters

Examples:

``` bash
wc -l notes.txt
wc -w notes.txt
wc -m notes.txt
```

Count the number of user accounts listed in `/etc/passwd`:

``` bash
wc -l /etc/passwd
```

## 6.8 Regular Expressions (Regex)

A **regular expression** is a pattern used to match text. Regex is
commonly used with `grep`, text editors, log analysis tools, and
programming languages.

Basic regular-expression symbols:

  -----------------------------------------------------------------------
  Symbol                  Meaning                 Example
  ----------------------- ----------------------- -----------------------
  `.`                     Any single character    `c.t` matches `cat`,
                                                  `cut`

  `^`                     Beginning of a line     `^root`

  `$`                     End of a line           `bash$`

  `*`                     Zero or more of the     `ab*c`
                          previous item           

  `[abc]`                 Any one listed          `[abc]at`
                          character               

  `[0-9]`                 Any digit               `[0-9]`

  `[^abc]`                Any character except    `[^0-9]`
                          listed characters       

  `+`                     One or more (with       `a+`
                          extended regex)         

  `?`                     Zero or one (with       `colou?r`
                          extended regex)         

  `\|`                    Alternation in basic    `error\|warning`
                          grep syntax when        
                          escaped                 
  -----------------------------------------------------------------------

With extended regular expressions (`grep -E`), alternation uses `|`
without escaping.

Examples:

Lines beginning with `root`:

``` bash
grep '^root' /etc/passwd
```

Lines ending in `bash`:

``` bash
grep 'bash$' /etc/passwd
```

Lines containing either `error` or `warning`:

``` bash
grep -E 'error|warning' application.log
```

Lines containing a three-digit number:

``` bash
grep -E '[0-9]{3}' file.txt
```

**Important:** Shell wildcard patterns and regular expressions are
related ideas, but they are not the same. For example, `*.txt` is a
shell glob pattern, while `.*\.txt$` is a regular expression that can
match a `.txt` suffix.

------------------------------------------------------------------------

# 7. Pipes and Redirection

## 7.1 What Is a Pipe?

A **pipe** sends the standard output of one command to the standard
input of another command.

The pipe symbol is:

``` text
|
```

Example:

``` bash
ls -l /etc | less
```

The output of `ls` is passed to `less`, allowing you to browse it page
by page.

Another example:

``` bash
ps aux | grep ssh
```

This searches the process listing for lines containing `ssh`.

A longer pipeline:

``` bash
cat /etc/passwd | cut -d: -f1 | sort
```

This: 1. Reads `/etc/passwd`. 2. Extracts the first colon-separated
field (user names). 3. Sorts the result.

You often do not need `cat` in a pipeline. This is simpler:

``` bash
cut -d: -f1 /etc/passwd | sort
```

## 7.2 Standard Input, Output, and Error

Linux processes commonly use three standard streams:

  Stream                         File descriptor Purpose
  ---------------------------- ----------------- -----------------------
  Standard input (`stdin`)                     0 Input to a command
  Standard output (`stdout`)                   1 Normal command output
  Standard error (`stderr`)                    2 Error messages

## 7.3 Output Redirection

`>` sends standard output to a file and overwrites the file if it
already exists.

``` bash
echo "Hello" > output.txt
```

`>>` appends standard output to a file:

``` bash
echo "Another line" >> output.txt
```

Redirect errors:

``` bash
ls /missing-directory 2> errors.txt
```

Redirect normal output and errors to the same file in Bash:

``` bash
command > output.txt 2>&1
```

Discard normal output:

``` bash
command > /dev/null
```

Discard error output:

``` bash
command 2> /dev/null
```

**Caution:** `>` overwrites the destination. Check the file name before
using it.

------------------------------------------------------------------------

# 8. Archiving and Compression

## 8.1 Archive vs. Compression

These terms describe different operations:

-   **Archiving** combines multiple files and directories into one
    container file.
-   **Compression** reduces the amount of storage space used by data.
-   Some formats support both, while others only compress a single
    stream or file.

For example, `tar` creates archives. `gzip` and `bzip2` compress data. A
`.tar.gz` file is a tar archive compressed with gzip.

## 8.2 `tar` --- Create and Extract Archives

`tar` stands for **Tape Archive**. It is commonly used to bundle files
and directories.

Create an uncompressed archive:

``` bash
tar -cvf backup.tar Documents/
```

Extract an archive:

``` bash
tar -xvf backup.tar
```

List the contents without extracting:

``` bash
tar -tvf backup.tar
```

Important options:

  Option     Description
  ---------- -----------------------------------------------
  `-c`       Create an archive
  `-x`       Extract an archive
  `-t`       List archive contents
  `-v`       Verbose output
  `-f`       Specify the archive file name
  `-z`       Use gzip compression
  `-j`       Use bzip2 compression
  `-J`       Use xz compression
  `-C DIR`   Change to DIR before performing the operation

Create a gzip-compressed archive:

``` bash
tar -czvf backup.tar.gz Documents/
```

Extract it:

``` bash
tar -xzvf backup.tar.gz
```

Create a bzip2-compressed archive:

``` bash
tar -cjvf backup.tar.bz2 Documents/
```

Extract it:

``` bash
tar -xjvf backup.tar.bz2
```

Extract into a specific directory:

``` bash
tar -xvf backup.tar -C /tmp/restore/
```

## 8.3 `gzip` --- Compress Files

`gzip` compresses files using the gzip format. By default, it replaces
the original file with a compressed `.gz` file.

``` bash
gzip report.txt
```

This normally creates `report.txt.gz`.

Decompress:

``` bash
gunzip report.txt.gz
```

Keep the original file while compressing:

``` bash
gzip -k report.txt
```

View compression information:

``` bash
gzip -l report.txt.gz
```

## 8.4 `bzip2` --- Compress Files

`bzip2` uses the bzip2 compression algorithm. It often compresses better
than gzip for some data, but may be slower.

``` bash
bzip2 report.txt
```

This normally creates `report.txt.bz2`.

Decompress:

``` bash
bunzip2 report.txt.bz2
```

Keep the original:

``` bash
bzip2 -k report.txt
```

## 8.5 `zip` and `unzip`

ZIP is a widely supported archive format that can bundle and compress
files.

Create a ZIP archive:

``` bash
zip backup.zip notes.txt report.txt
```

Add a directory recursively:

``` bash
zip -r website.zip website/
```

List archive contents:

``` bash
unzip -l backup.zip
```

Extract:

``` bash
unzip backup.zip
```

Extract to a particular directory:

``` bash
unzip backup.zip -d /tmp/restore/
```

## 8.6 Common Archive Formats

  Extension               Meaning
  ----------------------- -----------------------------------------
  `.tar`                  Tar archive; not necessarily compressed
  `.tar.gz` or `.tgz`     Tar archive compressed with gzip
  `.tar.bz2` or `.tbz2`   Tar archive compressed with bzip2
  `.tar.xz`               Tar archive compressed with xz
  `.gz`                   Gzip-compressed stream/file
  `.bz2`                  Bzip2-compressed stream/file
  `.zip`                  ZIP archive, usually compressed

------------------------------------------------------------------------

# 9. Important Linux Directories and Files

Linux organizes files in a single directory tree that begins at `/`,
called the **root directory**. Separate disks and partitions can be
mounted at different locations in this tree.

## 9.1 Important Directories

  -----------------------------------------------------------------------
  Directory                           Purpose
  ----------------------------------- -----------------------------------
  `/`                                 Root of the entire filesystem
                                      hierarchy

  `/home`                             Home directories for regular users

  `/root`                             Home directory of the root account

  `/etc`                              System-wide configuration files

  `/var`                              Variable data that changes during
                                      system operation

  `/var/log`                          System and application log files

  `/bin`                              Essential user commands; commonly
                                      linked to `/usr/bin` on modern
                                      systems

  `/sbin`                             Essential system administration
                                      commands; commonly linked to
                                      `/usr/sbin` on modern systems

  `/usr`                              Many user applications, libraries,
                                      documentation, and shared resources

  `/usr/bin`                          Many executable user commands

  `/usr/sbin`                         Many system administration
                                      executables

  `/lib` and `/lib64`                 Essential shared libraries and
                                      kernel-related modules on some
                                      systems; often linked into `/usr`
                                      on modern systems

  `/tmp`                              Temporary files; contents may be
                                      removed automatically

  `/dev`                              Device files representing hardware
                                      and virtual devices

  `/proc`                             Virtual filesystem exposing process
                                      and kernel information

  `/sys`                              Virtual filesystem exposing device
                                      and kernel information

  `/boot`                             Files needed to boot the system,
                                      such as kernels and bootloader data

  `/mnt`                              Common temporary mount point

  `/media`                            Common mount point for removable
                                      media

  `/opt`                              Optional third-party application
                                      software
  -----------------------------------------------------------------------

### Important distinction: `/` vs. `/root` vs. `/home`

-   `/` is the root of the entire filesystem.
-   `/root` is the home directory of the root administrator account.
-   `/home` usually contains the home directories of regular users, such
    as `/home/sina`.

## 9.2 `/etc/passwd` --- User Account Information

`/etc/passwd` contains account information for users known to the
system. Despite its name, modern systems normally do not store actual
password hashes in this file.

View it:

``` bash
cat /etc/passwd
```

Example entry:

``` text
sina:x:1000:1000:Sina User:/home/sina:/bin/bash
```

The fields are separated by colons:

1.  Username: `sina`
2.  Password placeholder: `x` (password hash is normally stored in
    `/etc/shadow`)
3.  UID: `1000`
4.  Primary GID: `1000`
5.  GECOS/comment field: `Sina User`
6.  Home directory: `/home/sina`
7.  Login shell: `/bin/bash`

## 9.3 `/etc/group` --- Group Information

`/etc/group` stores group definitions.

``` bash
cat /etc/group
```

Example:

``` text
students:x:1001:sina,ali
```

Fields:

1.  Group name: `students`
2.  Password placeholder: `x`
3.  GID: `1001`
4.  Supplementary group members: `sina,ali`

## 9.4 `/etc/shadow` --- Password Hash Information

`/etc/shadow` stores password hashes and password-aging information for
local accounts on systems using the traditional shadow-password
mechanism.

``` bash
sudo cat /etc/shadow
```

Access is restricted because this file contains sensitive authentication
data.

A line contains fields such as:

-   Username
-   Password hash or a marker indicating account state
-   Last password-change date
-   Minimum and maximum password age
-   Warning period
-   Inactivity period
-   Account expiration date

**Security note:** Never publish or casually share `/etc/shadow`.
Password hashes are sensitive and may be attacked offline if exposed.

## 9.5 `/var` and `/var/log`

`/var` stores data that changes as the system runs. Examples include
logs, caches, queues, and some application data.

`/var/log` commonly contains logs that help administrators troubleshoot
problems and investigate events.

Examples of possible log files:

``` bash
ls -lah /var/log
```

-   `auth.log` or `secure`: authentication-related events on some
    distributions.
-   `syslog` or `messages`: general system messages on some
    distributions.
-   `kern.log`: kernel messages on some distributions.

The exact files depend on the Linux distribution and logging
configuration.

Useful commands:

``` bash
sudo tail -f /var/log/auth.log
sudo less /var/log/syslog
```

On systems using systemd's journal:

``` bash
journalctl
journalctl -b
journalctl -u ssh
```

------------------------------------------------------------------------

# 10. Processes and System Information

## 10.1 What Is a Process?

A **process** is a running instance of a program. Each process has a
Process ID (PID). The operating system schedules processes and allocates
resources such as CPU time and memory.

## 10.2 `ps` --- Display Process Information

`ps` displays information about running processes.

``` bash
ps
```

Common options:

  -----------------------------------------------------------------------
  Option                              Description
  ----------------------------------- -----------------------------------
  `-e`                                Select all processes

  `-f`                                Full-format listing

  `-u USER`                           Show processes for a user

  `-p PID`                            Show a particular process

  `aux`                               Common BSD-style combination
                                      showing processes from all users
                                      with details
  -----------------------------------------------------------------------

Examples:

``` bash
ps aux
ps -ef
ps -u sina
ps -p 1234 -f
```

Useful fields in `ps aux` output:

-   `USER`: account that owns the process.
-   `PID`: process ID.
-   `%CPU`: CPU usage.
-   `%MEM`: memory usage.
-   `STAT`: process state.
-   `COMMAND`: command that started the process.

Find a process:

``` bash
ps aux | grep ssh
```

Note that the `grep` command itself may appear in the output. Tools such
as `pgrep` can be more convenient:

``` bash
pgrep -a ssh
```

## 10.3 `top` --- Real-Time Process Monitoring

`top` displays a live, updating view of processes and system resource
usage.

``` bash
top
```

It typically shows:

-   System uptime and load averages.
-   Number of running and sleeping tasks.
-   CPU usage.
-   Memory and swap usage.
-   Process IDs, owners, CPU usage, and memory usage.

Useful keys inside `top`:

  Key   Action
  ----- ---------------------------------------------------------
  `q`   Quit
  `P`   Sort by CPU usage
  `M`   Sort by memory usage
  `k`   Send a signal to a process (often used to terminate it)
  `1`   Toggle per-CPU display on many versions

## 10.4 `free` --- Display Memory Usage

`free` reports RAM and swap usage.

``` bash
free
```

Human-readable output:

``` bash
free -h
```

Important options:

  Option   Description
  -------- ------------------------------------
  `-h`     Human-readable units
  `-m`     Display in megabytes
  `-g`     Display in gigabytes
  `-s N`   Repeat the display every N seconds

Example:

``` bash
free -h
free -h -s 2
```

Linux uses otherwise-unused memory for caches and buffers. Therefore,
the `available` value is often more useful than simply looking at `free`
memory.

## 10.5 `dmesg` --- Display Kernel Messages

`dmesg` displays messages from the Linux kernel's ring buffer. These
messages can help troubleshoot hardware detection, drivers, devices, and
boot-related problems.

``` bash
dmesg
```

Useful options:

  Option          Description
  --------------- -----------------------------------------------------
  `-T`            Display human-readable timestamps where supported
  `-H`            Human-readable pager-style output
  `-w`            Wait for and display new messages
  `-l err,warn`   Filter by selected log levels on supported versions

Examples:

``` bash
dmesg -T | less
dmesg -T | grep -i usb
sudo dmesg -w
```

Some distributions restrict access to kernel messages for unprivileged
users.

------------------------------------------------------------------------

# 11. Linux Networking

Linux provides several tools for configuring interfaces, inspecting
routes, resolving names, and testing connectivity.

## 11.1 Network Interface Basics

A **network interface** is a connection point through which a system
communicates over a network.

Examples:

-   `eth0`, `enp0s3`: Ethernet interfaces.
-   `wlan0`, `wlp2s0`: Wireless interfaces.
-   `lo`: Loopback interface.

The loopback interface is used for communication with the local machine.
Its common IPv4 address is `127.0.0.1`.

## 11.2 `ifconfig` --- Display or Configure Network Interfaces

`ifconfig` is a traditional networking command from the older net-tools
package. It may not be installed by default on modern Linux systems.

Display active interfaces:

``` bash
ifconfig
```

Display all interfaces, including inactive ones:

``` bash
ifconfig -a
```

Display one interface:

``` bash
ifconfig eth0
```

Bring an interface up:

``` bash
sudo ifconfig eth0 up
```

Bring an interface down:

``` bash
sudo ifconfig eth0 down
```

Assign an IPv4 address and netmask:

``` bash
sudo ifconfig eth0 192.168.1.50 netmask 255.255.255.0
```

This is a legacy, temporary configuration method. The address may
disappear after a reboot or when a network-management service
reconfigures the interface.

### Modern Alternative: `ip`

The `ip` command from the iproute2 package is the modern standard for
many network tasks.

Show addresses:

``` bash
ip address
ip addr show
```

Show links:

``` bash
ip link show
```

Bring an interface up or down:

``` bash
sudo ip link set eth0 up
sudo ip link set eth0 down
```

Add an address:

``` bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

Remove an address:

``` bash
sudo ip addr del 192.168.1.50/24 dev eth0
```

Show the routing table:

``` bash
ip route
```

**Important:** The `ip` commands above change the current kernel network
configuration. Persistent configuration is normally managed by
NetworkManager, systemd-networkd, Netplan, or distribution-specific
configuration files.

## 11.3 `ping` --- Test Network Reachability

`ping` sends ICMP Echo Request messages to a destination and waits for
Echo Replies.

``` bash
ping 8.8.8.8
```

This can help determine whether a destination is reachable over the
network.

Important options:

  Option   Description
  -------- -------------------------------------------------------
  `-c N`   Send N packets, then stop
  `-i N`   Wait N seconds between packets
  `-W N`   Set a reply timeout in seconds on many Linux versions
  `-s N`   Set packet data size
  `-4`     Use IPv4
  `-6`     Use IPv6

Examples:

``` bash
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -4 -c 4 example.com
```

A successful ping shows that ICMP Echo traffic received replies. It does
**not** prove that every application or TCP/UDP port is working.

A failed ping does not always mean the destination is down. Firewalls
may block ICMP, or routing may be restricted.

## 11.4 `dig` --- Query DNS Information

`dig` stands for **Domain Information Groper**. It sends DNS queries and
displays DNS responses.

Basic query:

``` bash
dig example.com
```

Query a specific record type:

``` bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT
```

Common DNS record types:

  -----------------------------------------------------------------------
  Record                              Purpose
  ----------------------------------- -----------------------------------
  `A`                                 Maps a name to an IPv4 address

  `AAAA`                              Maps a name to an IPv6 address

  `MX`                                Identifies mail servers for a
                                      domain

  `NS`                                Identifies authoritative name
                                      servers

  `CNAME`                             Alias from one name to another

  `TXT`                               Stores text data, often used for
                                      verification and email security
                                      policies

  `PTR`                               Reverse DNS lookup
  -----------------------------------------------------------------------

Query a particular DNS server:

``` bash
dig @1.1.1.1 example.com
```

Request a short answer:

``` bash
dig +short example.com
```

Trace the delegation process:

``` bash
dig +trace example.com
```

Query reverse DNS:

``` bash
dig -x 8.8.8.8
```

## 11.5 `resolv.conf` --- DNS Resolver Configuration

`/etc/resolv.conf` is commonly used to configure the system's DNS
resolver.

View it:

``` bash
cat /etc/resolv.conf
```

Example:

``` text
nameserver 1.1.1.1
nameserver 8.8.8.8
search example.local
```

Common entries:

-   `nameserver`: DNS server address.
-   `search`: search domain appended to certain unqualified names.
-   `options`: resolver behavior settings.

**Important:** On many modern systems, `/etc/resolv.conf` is managed by
NetworkManager, systemd-resolved, or another service. Editing it
directly may be temporary or overwritten. Check the system's
network-management method before changing it.

Useful diagnostic commands:

``` bash
resolvectl status
getent hosts example.com
```

`getent hosts` asks the system's configured name-resolution system,
which may use more than DNS alone.

## 11.6 Additional Essential Networking Commands

### `ip` --- Addresses, Interfaces, and Routes

``` bash
ip addr
ip link
ip route
ip neigh
```

-   `ip addr`: IP addresses assigned to interfaces.
-   `ip link`: interface status and link-layer information.
-   `ip route`: routing table.
-   `ip neigh`: neighbor table, commonly used for ARP information on
    IPv4 and neighbor discovery on IPv6.

### `ss` --- Socket Statistics

`ss` displays socket and network connection information. It is a modern
replacement for many common `netstat` tasks.

``` bash
ss -tuln
```

Common options:

  Option   Description
  -------- -------------------------------------------
  `-t`     TCP sockets
  `-u`     UDP sockets
  `-l`     Listening sockets
  `-n`     Show numeric addresses and ports
  `-p`     Show associated processes where permitted
  `-a`     Show all sockets

Examples:

``` bash
ss -tuln
sudo ss -tulpn
ss -tan
```

### `traceroute` --- Trace the Network Path

`traceroute` attempts to show the routers (hops) between your computer
and a destination.

``` bash
traceroute example.com
```

Depending on the implementation, it may use UDP, ICMP, or TCP probes.
Firewalls can prevent some hops from responding.

### `hostname` --- Display or Set the Host Name

``` bash
hostname
hostname -I
hostnamectl
```

`hostname -I` displays configured IP addresses on many Linux
distributions. `hostnamectl` displays system host-name information and
can manage the static host name on systemd-based systems.

### `ip neigh` --- Inspect Neighbor/ARP Information

``` bash
ip neigh
```

This displays the local neighbor cache. For IPv4, it commonly contains
IP-to-MAC address mappings learned through ARP.

### `curl` --- Test Application-Level Connectivity

``` bash
curl -I https://example.com
```

`-I` requests response headers only. `curl` is useful for checking
whether an HTTP/HTTPS service responds; it is not simply a replacement
for `ping`.

### `wget` --- Retrieve Files Over HTTP/HTTPS/FTP

``` bash
wget https://example.com/file.zip
```

Useful for downloading files from a URL.

------------------------------------------------------------------------

# 12. Users and Groups

## 12.1 What Is a User?

A **user account** identifies an entity that can access a Linux system.
Each user has a UID (User ID), a primary group, a home directory, and
usually a login shell.

Linux systems commonly distinguish these broad account types:

### Root User

-   The root account has UID `0`.
-   It has extensive administrative privileges.
-   It can change system configuration, manage accounts, and access
    protected files.
-   Using root unnecessarily increases the risk of accidental damage or
    compromise.

### Standard User

-   A regular human account used for everyday work.
-   It normally has limited privileges.
-   It can use `sudo` for approved administrative tasks if configured.

### System User

-   An account used by a service or daemon, such as a web server or
    database service.
-   It may not have an interactive login shell.
-   It helps isolate services and limit their permissions.

The exact UID ranges and account conventions vary by distribution.

## 12.2 What Is a Group?

A **group** is a collection of user accounts. Groups make it easier to
manage access to files and resources.

For example, a directory can be owned by group `students`, and members
of that group can be granted shared access.

A user has:

-   One primary group.
-   Zero or more supplementary groups.

## 12.3 User vs. Group

  User                       Group
  -------------------------- --------------------------------------
  Represents an account      Represents a collection of accounts
  Has a UID                  Has a GID
  Owns processes and files   Can be assigned permissions on files
  Has a primary group        Can contain multiple users

## 12.4 `whoami` --- Show the Effective User Name

``` bash
whoami
```

It displays the user identity associated with the current effective user
ID.

Example:

``` text
sina
```

## 12.5 `id` --- Display UID, GID, and Groups

``` bash
id
```

Display information for a specific user:

``` bash
id sina
```

Example output:

``` text
uid=1000(sina) gid=1000(sina) groups=1000(sina),1001(students)
```

This means the user has UID 1000, primary GID 1000, and is also a member
of group `students`.

## 12.6 `sudo -i` --- Open a Root Login Shell

`sudo` runs a command with elevated privileges when the current user is
authorized.

``` bash
sudo command
```

`sudo -i` starts a login-style shell as root:

``` bash
sudo -i
```

The prompt often changes from `$` to `#`, although prompt appearance can
be customized.

Leave the root shell:

``` bash
exit
```

**Best practice:** Prefer running only the required command with `sudo`
rather than staying in a root shell.

Example:

``` bash
sudo apt update
```

## 12.7 `su` --- Switch User

`su` starts a shell as another user.

Switch to root:

``` bash
su -
```

Switch to a particular user:

``` bash
su - sina
```

The `-` requests a login shell, which loads the target user's login
environment.

On many distributions, root login may be disabled or unavailable. In
those cases, `sudo` is commonly used.

## 12.8 `adduser` --- Create a User Account

`adduser` is a friendly account-creation utility on many Debian-based
distributions. Its behavior differs across Linux distributions.

``` bash
sudo adduser ali
```

It may guide you through setting a password and entering account
details.

A lower-level alternative is `useradd`:

``` bash
sudo useradd -m ali
```

`-m` creates a home directory when appropriate.

## 12.9 `addgroup` --- Create a Group

On Debian-based systems:

``` bash
sudo addgroup students
```

A common lower-level alternative is:

``` bash
sudo groupadd students
```

## 12.10 `passwd` --- Set or Change Passwords

Change your own password:

``` bash
passwd
```

Set or reset another user's password as an administrator:

``` bash
sudo passwd ali
```

Lock an account's password:

``` bash
sudo passwd -l ali
```

Unlock it:

``` bash
sudo passwd -u ali
```

Locking a password does not necessarily disable every possible
authentication method or terminate existing sessions.

## 12.11 `usermod -a -G` --- Add a User to Supplementary Groups

`usermod` modifies an existing user account.

Add `sina` to the supplementary group `students`:

``` bash
sudo usermod -aG students sina
```

Meaning:

-   `-a`: append the group rather than replacing the existing
    supplementary group list.
-   `-G`: specify supplementary groups.

**Very important:** Do not omit `-a` when your intention is to add a
group. Using `-G` without `-a` can replace the user's existing
supplementary group memberships.

Check the result:

``` bash
id sina
groups sina
```

The new group membership may require the user to log out and back in
before it is reflected in existing sessions.

## 12.12 `userdel` --- Delete a User

Delete a user account:

``` bash
sudo userdel ali
```

Delete the account and its home directory:

``` bash
sudo userdel -r ali
```

**Caution:** `-r` can delete the user's home directory and mail spool.
Back up important data first and verify that the account is no longer
needed.

## 12.13 `groupdel` --- Delete a Group

``` bash
sudo groupdel students
```

A group cannot normally be deleted if it is the primary group of an
existing user. Check group membership and dependencies first.

## 12.14 `last` --- View Login History

`last` reads login records, commonly from `/var/log/wtmp`.

``` bash
last
```

Show a limited number of entries:

``` bash
last -n 10
```

Show records for one user:

``` bash
last sina
```

It can help administrators review recorded logins and system reboots.
Its output depends on available and retained login records; it is not a
complete security audit by itself.

------------------------------------------------------------------------

# 13. Linux Permissions and Ownership

## 13.1 Why Does Linux Use Permissions?

Linux permissions control who can read, modify, or execute files and
directories.

For each filesystem object, permissions are commonly described for three
classes:

1.  **User (u):** the owner of the file.
2.  **Group (g):** the file's owning group.
3.  **Other (o):** everyone else.

## 13.2 The Three Basic Permissions

  -----------------------------------------------------------------------
  Permission              File meaning            Directory meaning
  ----------------------- ----------------------- -----------------------
  `r` (read)              Read file contents      List directory entries
                                                  (names)

  `w` (write)             Modify file contents    Create, delete, or
                                                  rename entries, subject
                                                  to other controls

  `x` (execute)           Execute the file, if it Traverse/search the
                          is executable           directory
  -----------------------------------------------------------------------

**Important directory detail:** To access a file through a directory
path, the user generally needs execute (`x`) permission on the relevant
directories. Reading a directory's names and accessing an item inside it
are different operations.

## 13.3 Understanding Permission Output

Example:

``` text
-rwxr-xr--
```

Breakdown:

-   First character `-`: regular file.
-   `rwx`: owner can read, write, and execute.
-   `r-x`: group can read and execute.
-   `r--`: others can only read.

For a directory, the first character is usually `d`.

## 13.4 Numeric (Octal) Permissions

Each basic permission has a numeric value:

  Permission        Value
  --------------- -------
  Read (`r`)            4
  Write (`w`)           2
  Execute (`x`)         1

Add the values for each class:

    Number Permissions
  -------- -------------
         0 `---`
         1 `--x`
         2 `-w-`
         3 `-wx`
         4 `r--`
         5 `r-x`
         6 `rw-`
         7 `rwx`

Example:

``` text
755 = 7 5 5
      rwx r-x r-x
```

This means: - Owner: read, write, execute. - Group: read, execute. -
Others: read, execute.

Another example:

``` text
644 = rw- r-- r--
```

The owner can read and write; everyone else can only read.

## 13.5 `chmod` --- Change File Permissions

`chmod` changes the permission mode of a file or directory.

### Numeric Mode

``` bash
chmod 755 script.sh
chmod 644 notes.txt
chmod 700 private-directory
```

### Symbolic Mode

Symbolic mode uses:

-   `u`: user/owner
-   `g`: group
-   `o`: others
-   `a`: all
-   `+`: add permission
-   `-`: remove permission
-   `=`: set exact permission for the selected class

Examples:

``` bash
chmod u+x script.sh
chmod g-w shared.txt
chmod o-r secret.txt
chmod u=rw,g=r,o= file.txt
chmod a+r public.txt
```

### Recursive Permissions

``` bash
chmod -R 755 public-directory/
```

This applies the mode recursively to the directory and its contents.

**Caution:** Applying the same permission mode recursively to every file
and directory is often a mistake. Directories usually need execute
permission for traversal, while ordinary files often do not need it.
Consider targeted commands or `find` when different modes are required.

## 13.6 `chown` --- Change File Owner and Group

`chown` changes ownership.

Change the owner:

``` bash
sudo chown ali notes.txt
```

Change owner and group:

``` bash
sudo chown ali:students notes.txt
```

Change only the group:

``` bash
sudo chown :students notes.txt
```

Apply recursively:

``` bash
sudo chown -R ali:students project/
```

The ability to change ownership is restricted and normally requires
administrator privileges.

## 13.7 `chgrp` --- Change Group Ownership

`chgrp` changes the group associated with a file or directory.

``` bash
sudo chgrp students notes.txt
```

Recursive example:

``` bash
sudo chgrp -R students project/
```

Difference:

-   `chown`: changes owner and/or group.
-   `chgrp`: changes group ownership.

## 13.8 Special Permissions

Linux has three special permission bits:

1.  SUID (Set User ID)
2.  SGID (Set Group ID)
3.  Sticky Bit

These are additional permission mechanisms, often used for specific
system and shared-directory behaviors.

### SUID --- Set User ID

When set on an executable file, SUID generally causes the program to run
with the effective user ID of the file's owner rather than the user who
launched it.

Numeric value: `4` in the special-permission digit.

Example:

``` bash
sudo chmod 4755 program
```

In a long listing, SUID is represented by `s` in the owner's execute
position:

``` text
-rwsr-xr-x
```

If the owner-execute bit is not set, a capital `S` may appear.

**Security note:** SUID programs require careful review because a
vulnerability in a privileged executable can have serious consequences.
SUID behavior is generally meaningful for executable files; it is not a
way to grant ordinary file access.

### SGID --- Set Group ID

On an executable file, SGID generally causes the process to use the
file's group as its effective group.

On a directory, SGID commonly causes newly created files and
subdirectories to inherit the directory's group. New subdirectories also
commonly inherit the SGID bit.

Numeric value: `2` in the special-permission digit.

Example:

``` bash
sudo chmod 2775 shared/
```

A directory listing may show:

``` text
drwxrwsr-x
```

This is useful for shared team directories.

### Sticky Bit

The sticky bit is commonly used on shared directories such as `/tmp`.

When set on a directory, users generally cannot delete or rename files
inside it unless they own the file, own the directory, or have the
relevant privilege.

Numeric value: `1` in the special-permission digit.

Set the sticky bit:

``` bash
sudo chmod 1777 shared-temp/
```

A common system example is:

``` text
drwxrwxrwt
```

The final `t` represents the sticky bit.

### Quick Comparison

  ------------------------------------------------------------------------
  Special bit                          Numeric value Main purpose
  --------------------- ---------------------------- ---------------------
  SUID                                             4 Executable uses file
                                                     owner's effective UID

  SGID                                             2 Executable uses file
                                                     group's effective
                                                     GID; on directories,
                                                     supports group
                                                     inheritance

  Sticky bit                                       1 Restricts
                                                     deletion/renaming of
                                                     entries in shared
                                                     directories
  ------------------------------------------------------------------------

Examples:

``` bash
chmod 4755 program
chmod 2775 shared/
chmod 1777 shared-temp/
```

------------------------------------------------------------------------

# 14. Symbolic Links

## 14.1 What Is a Link?

A link is a filesystem reference to another file or directory. Linux
supports hard links and symbolic (soft) links.

## 14.2 `ln -s` --- Create a Symbolic Link

A **symbolic link** is a special file that points to another path.

Syntax:

``` bash
ln -s TARGET LINK_NAME
```

Example:

``` bash
ln -s /var/log/system.log system-log
```

This creates a symbolic link named `system-log` pointing to
`/var/log/system.log`.

Create a symbolic link to a directory:

``` bash
ln -s /home/sina/Documents ~/Documents-link
```

View links:

``` bash
ls -l
```

The output may look like:

``` text
system-log -> /var/log/system.log
```

### Absolute vs. Relative Link Targets

Absolute target:

``` bash
ln -s /home/sina/Documents docs-link
```

Relative target:

``` bash
ln -s ../Documents docs-link
```

A relative symbolic-link target is interpreted relative to the directory
containing the link, not necessarily the directory from which you typed
the command.

### Important Characteristics of Symbolic Links

-   A symbolic link has its own directory entry and inode.
-   It can point to files or directories.
-   It can point to a target on another filesystem.
-   It can become a broken (dangling) link if the target is moved or
    deleted.
-   Accessing the link normally follows the target path.

Find symbolic links:

``` bash
find /home/sina -type l
```

Remove a symbolic link:

``` bash
rm docs-link
```

This removes the link itself, not the target.

## 14.3 Hard Links vs. Symbolic Links

Although your notes specifically mention `ln -s`, understanding hard
links is useful.

A hard link is another directory entry referring to the same inode (the
same underlying filesystem object).

Create a hard link:

``` bash
ln original.txt hardlink.txt
```

  -----------------------------------------------------------------------
  Symbolic link                       Hard link
  ----------------------------------- -----------------------------------
  Points to a path                    Refers to the same inode

  Can point to directories (normally) Normally cannot be created for
                                      directories by ordinary users

  Can cross filesystem boundaries     Must be on the same filesystem

  Can become dangling                 Remains valid while the inode still
                                      has a link

  Has its own inode                   Shares the target inode
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 15. Additional Useful Commands

The following commands were not all in the original notes, but they are
valuable for Linux administration, networking, and security
fundamentals.

## 15.1 `uname` --- Kernel and System Information

``` bash
uname -a
uname -r
```

-   `-a`: display broad system information.
-   `-r`: display the kernel release.

## 15.2 `hostnamectl` --- Host Name and Operating System Details

``` bash
hostnamectl
```

On systemd-based distributions, it displays information such as host
name, operating system, kernel, and architecture.

## 15.3 `df` --- Disk Space Usage

``` bash
df -h
```

Displays filesystem space usage.

Useful options:

-   `-h`: human-readable sizes.
-   `-T`: show filesystem type.
-   `-i`: show inode usage.

Example:

``` bash
df -hT
```

## 15.4 `du` --- Directory and File Space Usage

``` bash
du -sh /var/log
```

-   `-s`: summary for the specified path.
-   `-h`: human-readable units.

Show the sizes of immediate subdirectories:

``` bash
du -h --max-depth=1 /home
```

## 15.5 `which` and `type` --- Locate Commands

``` bash
which python3
type cd
type ls
```

`which` searches the `PATH` for an executable. `type` can identify
aliases, shell functions, built-ins, and executable paths.

## 15.6 `history` --- View Command History

``` bash
history
```

Run a previous command by its history number:

``` bash
!25
```

Search interactively through Bash history with `Ctrl + R`.

Be careful: shell history may contain sensitive information if commands
included passwords or tokens.

## 15.7 `kill` and `pkill` --- Send Signals to Processes

`kill` sends a signal to a process, identified by PID.

``` bash
kill 1234
```

The default signal is usually `SIGTERM` (15), which asks the process to
terminate gracefully.

Force termination:

``` bash
kill -9 1234
```

`-9` sends `SIGKILL`. The process cannot handle or ignore this signal,
so use it only when normal termination fails.

Find and signal processes by name:

``` bash
pgrep -a ssh
pkill -TERM process-name
```

## 15.8 `systemctl` --- Manage systemd Services

On systems using systemd:

``` bash
systemctl status ssh
```

Common operations:

``` bash
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl enable ssh
sudo systemctl disable ssh
```

-   `start`: start now.
-   `stop`: stop now.
-   `restart`: restart now.
-   `enable`: configure service to start automatically at boot.
-   `disable`: remove automatic startup configuration.

Service names differ by distribution. For example, an SSH server service
may be called `ssh` or `sshd`.

## 15.9 `journalctl` --- Read systemd Logs

``` bash
journalctl
journalctl -b
journalctl -u ssh
journalctl -p warning
journalctl -f
```

-   `-b`: current boot.
-   `-u`: filter by service unit.
-   `-p`: filter by priority.
-   `-f`: follow new log entries.

## 15.10 `curl` --- Test Web Services

``` bash
curl -I https://example.com
curl -v https://example.com
```

`-v` shows detailed connection and protocol information. Be careful when
sharing verbose output because it may contain sensitive headers or other
information.

## 15.11 `nc` (Netcat) --- Test TCP/UDP Connections

Netcat can read and write data over network connections. It is useful
for basic connectivity tests and troubleshooting.

Test whether a TCP port is reachable:

``` bash
nc -vz example.com 443
```

-   `-v`: verbose output.
-   `-z`: scan for listening services without sending application data
    (behavior varies by implementation).

Use it only against systems you own or are authorized to test.

## 15.12 `nmap` --- Network Discovery and Port Scanning

`nmap` is a network discovery and security auditing tool.

Basic scan:

``` bash
nmap 192.168.1.10
```

Scan selected ports:

``` bash
nmap -p 22,80,443 192.168.1.10
```

Scan a host for common TCP ports:

``` bash
nmap -sT 192.168.1.10
```

-   `-p`: select ports.
-   `-sT`: TCP connect scan.

Only scan networks and devices you own or have explicit permission to
assess. Scanning can trigger security alerts and may violate policies on
networks you do not administer.

------------------------------------------------------------------------

# 16. Practical Exercises

Use a Linux virtual machine or a system where you have permission to
experiment. Avoid practicing destructive commands on important files.

## Exercise 1: Navigation

1.  Display your current directory.
2.  Go to `/home`.
3.  List all files, including hidden files.
4.  Go back to your home directory.
5.  Navigate to `/etc` using an absolute path.

Suggested commands:

``` bash
pwd
cd /home
ls -la
cd ~
cd /etc
```

## Exercise 2: File Management

1.  Create a directory called `linux-practice`.
2.  Enter the directory.
3.  Create `notes.txt`.
4.  Add a line of text using Nano.
5.  Display the file.
6.  Copy it to `notes-backup.txt`.
7.  Rename the copy to `backup.txt`.

Suggested commands:

``` bash
mkdir linux-practice
cd linux-practice
touch notes.txt
nano notes.txt
cat notes.txt
cp notes.txt notes-backup.txt
mv notes-backup.txt backup.txt
```

## Exercise 3: Text Processing

Create a file containing several lines, including the words `error`,
`warning`, and `success`.

Then:

1.  Display the first three lines.
2.  Display the last three lines.
3.  Search for `error`.
4.  Search without case sensitivity.
5.  Count the lines.
6.  Sort the file.

Suggested commands:

``` bash
head -n 3 sample.log
tail -n 3 sample.log
grep "error" sample.log
grep -i "ERROR" sample.log
wc -l sample.log
sort sample.log
```

## Exercise 4: Users and Groups

On a disposable test VM:

1.  Create a group named `network-lab`.
2.  Create a user named `student1`.
3.  Add the user to `network-lab`.
4.  Check the user's UID and group membership.
5.  Change the user's password.

Suggested commands (distribution-dependent):

``` bash
sudo groupadd network-lab
sudo useradd -m student1
sudo usermod -aG network-lab student1
id student1
sudo passwd student1
```

## Exercise 5: Permissions

1.  Create `private.txt`.
2.  Set its permissions to `600`.
3.  Inspect the permissions.
4.  Add execute permission for the owner.
5.  Remove read permission for others.

Suggested commands:

``` bash
touch private.txt
chmod 600 private.txt
ls -l private.txt
chmod u+x private.txt
chmod o-r private.txt
```

## Exercise 6: Networking

1.  Display your IP addresses.
2.  Display your network interfaces.
3.  Display the routing table.
4.  Ping your default gateway (find its address from the routing table).
5.  Query the A record of `example.com`.
6.  Display listening TCP and UDP sockets.

Suggested commands:

``` bash
ip addr
ip link
ip route
ping -c 4 GATEWAY_IP
dig example.com A
ss -tuln
```

Replace `GATEWAY_IP` with the actual gateway address shown by
`ip route`.

------------------------------------------------------------------------

# 17. Safety Notes and Common Mistakes

1.  **Be careful with `rm -rf`.** It can recursively and permanently
    delete data. Verify the target path before execution.
2.  **Do not work as root unnecessarily.** Use `sudo` for specific
    administrative commands.
3.  **Do not confuse `/` with `/root`.** `/` is the filesystem root;
    `/root` is the root user's home directory.
4.  **Do not omit `-a` from `usermod -aG` when adding a supplementary
    group.** Without `-a`, existing supplementary group memberships may
    be replaced.
5.  **Do not assume `ifconfig` is the modern standard.** Learn
    `ip addr`, `ip link`, and `ip route` as well.
6.  **Do not assume a failed ping proves a host is offline.** ICMP may
    be blocked while other services remain available.
7.  **Do not assume `/etc/resolv.conf` is permanently editable.** A
    network-management service may generate or replace it.
8.  **Do not apply recursive permissions blindly.** Files and
    directories often need different permissions.
9.  **Protect `/etc/shadow` and sensitive logs.** They may contain
    authentication-related information.
10. **Check command help and manual pages.** Options can differ across
    distributions and implementations.
11. **Understand redirection.** `>` overwrites a file; `>>` appends to
    it.
12. **Use authorized lab environments for security tools.** Do not scan
    or test networks without permission.

------------------------------------------------------------------------

# Quick Reference: Frequently Used Commands

  Task                     Command
  ------------------------ -------------------------------
  Show current directory   `pwd`
  List files               `ls -lah`
  Change directory         `cd PATH`
  Create file              `touch FILE`
  Create directory         `mkdir DIR`
  Edit text file           `nano FILE`
  Display file             `cat FILE`
  Read long file           `less FILE`
  Copy file                `cp SOURCE DEST`
  Move/rename file         `mv SOURCE DEST`
  Delete file              `rm FILE`
  Delete empty directory   `rmdir DIR`
  Search text              `grep PATTERN FILE`
  Find files               `find PATH ...`
  Show first lines         `head FILE`
  Show last lines          `tail FILE`
  Follow a log             `tail -f FILE`
  Show processes           `ps aux`
  Monitor processes        `top`
  Show memory              `free -h`
  Show kernel messages     `dmesg -T`
  Show IP addresses        `ip addr`
  Show routes              `ip route`
  Test reachability        `ping -c 4 HOST`
  Query DNS                `dig DOMAIN`
  Show sockets             `ss -tuln`
  Show current user        `whoami`
  Show user IDs/groups     `id`
  Change password          `passwd`
  Add user to group        `sudo usermod -aG GROUP USER`
  Change permissions       `chmod MODE FILE`
  Change owner/group       `chown USER:GROUP FILE`
  Change group             `chgrp GROUP FILE`
  Create symbolic link     `ln -s TARGET LINK`
  Read manual              `man COMMAND`

------------------------------------------------------------------------

**End of Guide**

This guide is a practical foundation, not a replacement for
distribution-specific documentation. When working on a real system,
check the installed command's manual page and test unfamiliar operations
in a safe lab environment.
