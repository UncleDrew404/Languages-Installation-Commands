# LINUX

A complete Linux command guide, from the basics to master level usage. It covers navigation, file management, text searching, permissions, package management, remote access, Git, process control, scripting, and advanced one-liners.

## Table of Contents

1. [Introduction](#introduction)
2. [Before You Start](#before-you-start)
3. [Navigation: pwd, ls, cd](#navigation-pwd-ls-cd)
4. [File and Directory Management](#file-and-directory-management)
5. [Viewing File Contents](#viewing-file-contents)
6. [Searching and Text Processing: grep](#searching-and-text-processing-grep)
7. [Permissions and Ownership](#permissions-and-ownership)
8. [Superuser Access: sudo](#superuser-access-sudo)
9. [Package Management: apt](#package-management-apt)
10. [Remote Access: ssh](#remote-access-ssh)
11. [Version Control: git](#version-control-git)
12. [Process Management](#process-management)
13. [Disks and Storage](#disks-and-storage)
14. [Archives and Compression](#archives-and-compression)
15. [Users and Groups](#users-and-groups)
16. [Environment and Shell Customization](#environment-and-shell-customization)
17. [Redirection, Pipes, and Wildcards](#redirection-pipes-and-wildcards)
18. [Scheduling Tasks](#scheduling-tasks)
19. [Bash Scripting: Basic to Master](#bash-scripting-basic-to-master)
20. [Master Level Tools and One-Liners](#master-level-tools-and-one-liners)
21. [Cheatsheet](#cheatsheet)
22. [Best Practices](#best-practices)

## Introduction

Linux is a free and open-source operating system kernel created by Linus Torvalds in 1991. Combined with tools from the GNU project, it powers servers, cloud platforms, Android devices, embedded systems, and desktop distributions.

Common distributions:

| Distribution | Package Manager | Notes |
|:---|:---|:---|
| Ubuntu / Debian / Mint | `apt` | Most common for servers and beginners |
| Fedora / RHEL / CentOS | `dnf` | Enterprise and cutting edge |
| Arch Linux / Manjaro | `pacman` | Rolling release, minimal by default |
| openSUSE | `zypper` | Enterprise and desktop |
| Alpine | `apk` | Containers and minimal systems |

This guide uses Ubuntu / Debian commands for package management, but almost every other command works the same on all distributions.

### The Shell

The shell is the program that reads the commands you type and asks the system to run them.

| Shell | Description |
|:---|:---|
| `bash` | Default on most Linux systems |
| `zsh` | Popular interactive shell with themes and plugins |
| `sh` | Minimal POSIX shell, used for portable scripts |
| `fish` | User friendly shell with autosuggestions |

### Command Syntax

```
command [options] [arguments]
```

Example: `ls -l /home` runs the command `ls`, with option `-l`, on the argument `/home`.

Rules to remember:

- Linux is case sensitive: `File.txt` and `file.txt` are different files.
- Paths starting with `/` are absolute: `/etc/hosts`.
- Paths without a leading `/` are relative to the current directory: `docs/readme.md`.
- `~` means your home directory, `.` means the current directory, `..` means the parent directory.
- Options usually start with `-` (short form) or `--` (long form), for example `-a` and `--all`.
- Most commands accept `--help`, for example `cp --help`.

### Getting Help

| Command | Description |
|:---|:---|
| `man ls` | Opens the full manual page for a command. Press `q` to quit, `/word` to search. |
| `ls --help` | Shows a quick summary of options. |
| `info ls` | Opens the GNU info documentation when available. |
| `apropos copy` | Searches man pages by keyword. |
| `whatis ls` | Shows a one line description of a command. |
| `type ls` | Shows whether a name is a builtin, alias, function, or file. |
| `which python3` | Shows the path of the program that will run. |
| `history` | Shows previously run commands. |

### Useful First Commands

| Command | Description |
|:---|:---|
| `whoami` | Shows the current user name. |
| `hostname` | Shows the machine name. |
| `uname -a` | Shows kernel and system information. |
| `date` | Shows the current date and time. |
| `uptime` | Shows how long the system has been running and the load average. |
| `clear` | Clears the terminal screen. |
| `exit` | Closes the current shell or SSH session. |
| `sudo reboot` | Restarts the machine. |
| `sudo shutdown -h now` | Shuts down the machine immediately. |

### Keyboard Shortcuts in the Terminal

| Shortcut | Action |
|:---|:---|
| `Tab` | Autocomplete a file, directory, or command. Press twice to list matches. |
| `Ctrl+C` | Cancel the running command. |
| `Ctrl+D` | End input or close the shell. |
| `Ctrl+L` | Clear the screen, same as `clear`. |
| `Ctrl+R` | Search backward through command history. |
| `Ctrl+A` / `Ctrl+E` | Move the cursor to the start or end of the line. |
| `Ctrl+U` / `Ctrl+K` | Delete from the cursor to the start or end of the line. |
| `Ctrl+W` | Delete the word before the cursor. |
| `Ctrl+Z` | Suspend the running command and send it to the background. |
| `!!` | Repeats the previous command, for example `sudo !!`. |

## Navigation: pwd, ls, cd

These three commands are how you move around the filesystem.

### pwd

`pwd` (print working directory) shows the absolute path of the directory you are currently in.

```
pwd
```

| Option | Description |
|:---|:---|
| `pwd -L` | Shows the logical path, keeping symbolic links (default). |
| `pwd -P` | Shows the physical path with symbolic links resolved. |

```
cd /var/log
pwd            # /var/log
pwd -P         # resolves symlinks, for example /private/var/log on some systems
```

### ls

`ls` (list) shows the contents of a directory.

```
ls [options] [path]
```

| Option | Description |
|:---|:---|
| `ls` | Lists names in the current directory. |
| `ls -l` | Long format: permissions, owner, group, size, and date. |
| `ls -a` | Shows all files, including hidden files that start with a dot. |
| `ls -A` | Like `-a` but hides `.` and `..`. |
| `ls -h` | Human readable sizes, for example `4.0K`, `2.1M`. |
| `ls -R` | Lists subdirectories recursively. |
| `ls -t` | Sorts by modification time, newest first. |
| `ls -S` | Sorts by file size, largest first. |
| `ls -r` | Reverses the sort order. |
| `ls -i` | Shows inode numbers. |
| `ls -d */` | Lists only directories. |
| `ls -1` | One entry per line. |
| `ls --color=auto` | Colorizes output by file type. |

Examples:

```
ls                    # basic listing
ls -la                # all files, long format
ls -lh /var/log       # readable sizes
ls -lt                # newest files first
ls -lS | head -5      # five largest entries (pipe)
ls -R src             # recursive listing of the src directory
ls *.md               # wildcard: all markdown files
ls -d */              # only directories
```

Reading `ls -l` output:

```
-rw-r--r-- 1 alice dev  4096 Oct  2 10:30 notes.txt
|└┬┘└┬┘└┬┘ |  |    |     |       |
| |  |  |  |  |    |     |       └── name
| |  |  |  |  |    |     └────────── modification time
| |  |  |  |  |    └──────────────── size in bytes
| |  |  |  |  └───────────────────── group
| |  |  |  └──────────────────────── owner
| |  |  └─────────────────────────── others permissions
| |  └────────────────────────────── group permissions
└─┴───────────────────────────────── file type and owner permissions
```

### cd

`cd` (change directory) moves you to another directory.

```
cd [path]
```

| Command | Description |
|:---|:---|
| `cd` or `cd ~` | Goes to your home directory. |
| `cd /` | Goes to the root of the filesystem. |
| `cd ..` | Goes up one level. |
| `cd ../..` | Goes up two levels. |
| `cd -` | Goes back to the previous directory. |
| `cd /etc/nginx` | Goes to an absolute path. |
| `cd projects/api` | Goes to a relative path. |
| `cd "My Folder"` | Uses quotes when the name contains spaces. |
| `cd ~bob` | Goes to the home directory of user `bob`. |

Examples:

```
cd /var/www/html
cd ../../etc
cd -                  # jump back to where you were
cd                    # back to home
```

Tip: press `Tab` while typing a path to autocomplete it, and press `Tab` twice to see all matching names.

## File and Directory Management

### mkdir

`mkdir` (make directory) creates new directories.

```
mkdir [options] directory...
```

| Option | Description |
|:---|:---|
| `-p` | Creates parent directories as needed and does not fail if the directory exists. |
| `-m 755` | Sets permissions at creation time. |
| `-v` | Prints a message for every directory created. |

Examples:

```
mkdir projects
mkdir -p projects/api/src          # creates the whole chain
mkdir -p project/{src,docs,tests}  # brace expansion creates three directories
mkdir -m 700 private                # only the owner can access it
mkdir -pv logs/2026/10
```

### touch

`touch` creates empty files or updates timestamps of existing files.

```
touch file.txt
touch file1.txt file2.txt
touch -t 202610021030 file.txt   # set a specific timestamp YYYYMMDDhhmm
touch -c file.txt                # do not create the file if it does not exist
```

### cp

`cp` (copy) copies files and directories. The original stays in place.

```
cp [options] source destination
```

| Option | Description |
|:---|:---|
| `-r` / `-R` | Copies directories recursively. |
| `-i` | Asks before overwriting an existing file. |
| `-n` | Never overwrites an existing file. |
| `-u` | Copies only when the source is newer than the destination. |
| `-a` | Archive mode: recursive plus preserves permissions, links, and timestamps. |
| `-p` | Preserves permissions, ownership, and timestamps. |
| `-v` | Shows what is being copied. |
| `-L` | Follows symbolic links and copies the target content. |
| `--parents` | Recreates the source path structure inside the destination. |

Examples:

```
cp notes.txt notes.bak            # copy a file
cp notes.txt /tmp/                # copy into a directory
cp -r src/ backup/                # copy a directory recursively
cp -a /etc/nginx /backup/nginx    # perfect copy for backups
cp -u *.conf /etc/app/            # copy only changed files
cp -i important.txt /mnt/usb/     # ask before overwrite
cp -v report.pdf ~/Documents/     # verbose copy
cp --parents src/api/app.py /backup   # creates /backup/src/api/app.py
```

### mv

`mv` (move) moves or renames files and directories. Unlike `cp`, the original is removed from its old location.

```
mv [options] source destination
```

| Option | Description |
|:---|:---|
| `-i` | Asks before overwriting. |
| `-n` | Never overwrites an existing file. |
| `-u` | Moves only when the source is newer. |
| `-v` | Shows what is being moved. |
| `-b` | Creates a backup of the overwritten file. |

Examples:

```
mv oldname.txt newname.txt        # rename a file
mv notes.txt ~/Documents/         # move a file
mv -i a.txt b.txt                 # ask before replacing b.txt
mv src/ /mnt/backup/              # move a directory
mv -v *.log logs/                 # move all log files with output
for f in *.jpeg; do mv "$f" "${f%.jpeg}.jpg"; done   # bulk rename
```

Note: within the same filesystem `mv` is instant because only the pointer changes. Across filesystems it copies then deletes, which can take time.

### rm

`rm` (remove) deletes files and directories. There is no trash can by default, so deleted data is hard to recover.

```
rm [options] file...
```

| Option | Description |
|:---|:---|
| `-i` | Asks for confirmation before every removal. |
| `-f` | Forces removal, ignores missing files and prompts. |
| `-r` / `-R` | Removes directories and their contents recursively. |
| `-d` | Removes empty directories. |
| `-v` | Shows what is being removed. |
| `--preserve-root` | Refuses to remove `/` (default on modern systems). |

Examples:

```
rm file.txt                       # delete one file
rm -i important.txt               # ask first
rm -r old_project/                # delete a directory and its contents
rm -rf node_modules/              # force recursive delete, no prompts
rm -v *.tmp                       # verbose delete of temp files
rm -- -weird-name.txt             # use -- before names starting with a dash
```

Safety rules:

- Never run `rm -rf $VAR/` when `VAR` may be empty, because it becomes `rm -rf /`. Use `rm -rf "${VAR:?}/"` instead.
- Prefer `trash-cli` (`sudo apt install trash-cli`) or move files to a temporary folder when unsure.
- Double check the path before pressing Enter, especially with `-rf`.
- `rm -rf /` and `--no-preserve-root` can destroy the system. Treat them as forbidden.

### ln

`ln` creates links between files.

| Command | Description |
|:---|:---|
| `ln target linkname` | Hard link: another name for the same data, no extra space used. |
| `ln -s target linkname` | Symbolic (soft) link: a shortcut pointing to a path. |
| `ln -sf target linkname` | Force replacement of an existing symlink. |

```
ln -s /var/www/app/current release   # symlink to a directory
ln -s /usr/bin/python3 /usr/local/bin/python
ls -l release                        # shows release -> /var/www/app/current
```

### Other File Inspection Commands

| Command | Description |
|:---|:---|
| `file photo.jpg` | Shows the real type of a file, not just the extension. |
| `stat notes.txt` | Shows size, permissions, inode, and access times. |
| `tree -L 2` | Draws a directory tree two levels deep (install with `sudo apt install tree`). |
| `basename /etc/nginx/nginx.conf` | Prints `nginx.conf`. |
| `dirname /etc/nginx/nginx.conf` | Prints `/etc/nginx`. |
| `readlink -f link` | Prints the final target of a symlink. |
## Viewing File Contents

### cat

`cat` (concatenate) prints the contents of one or more files to the terminal. It is best for short files.

```
cat [options] file...
```

| Option | Description |
|:---|:---|
| `-n` | Numbers all output lines. |
| `-b` | Numbers only non empty lines. |
| `-s` | Squeezes multiple blank lines into one. |
| `-A` | Shows hidden characters: tabs as `^I` and line ends as `$`. |
| `-E` | Shows `$` at the end of every line. |

Examples:

```
cat notes.txt                     # print a file
cat -n script.sh                  # print with line numbers
cat file1.txt file2.txt           # print two files in order
cat file1.txt file2.txt > all.txt # join files into a new one
cat > newfile.txt                 # create a file, type content, press Ctrl+D to finish
cat >> notes.txt                  # append typed content to a file
cat /dev/null > app.log           # empty a log file without deleting it
cat config.yml | grep port        # pipe content into grep (useful pattern)
```

Heredoc, a common way to write multi line content from a script:

```
cat << EOF > config.ini
[server]
port = 8080
EOF
```

Tip: for long files use `less` instead of `cat`, because `cat` floods the terminal.

### less and more

`less` opens a file in a scrollable pager without loading everything into memory.

```
less /var/log/syslog
```

| Key | Action |
|:---|:---|
| `Space` / `b` | Page down / page up. |
| `j` / `k` | Scroll one line down / up. |
| `/word` | Search forward, press `n` for the next match. |
| `?word` | Search backward. |
| `g` / `G` | Jump to the start / end of the file. |
| `q` | Quit. |

| Option | Description |
|:---|:---|
| `less -N file` | Shows line numbers. |
| `less -S file` | Truncates long lines instead of wrapping. |
| `less +F file` | Follows the file like `tail -f`, press `Ctrl+C` to return to navigation. |

### head and tail

`head` shows the first lines of a file, `tail` shows the last lines. Both default to 10 lines.

```
head -n 20 access.log             # first 20 lines
tail -n 50 access.log             # last 50 lines
tail -f /var/log/nginx/error.log  # follow new lines live, Ctrl+C to stop
tail -F app.log                   # like -f but survives log rotation
tail -f app.log | grep ERROR      # watch only errors
head -c 100 file.bin              # first 100 bytes
```

### wc, nl, tac, rev

| Command | Description |
|:---|:---|
| `wc -l file` | Counts lines. |
| `wc -w file` | Counts words. |
| `wc -c file` | Counts bytes. |
| `nl file` | Numbers lines, similar to `cat -n`. |
| `tac file` | Prints a file in reverse line order. |
| `rev file` | Reverses the characters in every line. |

```
wc -l *.log                       # line count for every log file
grep -c ERROR app.log             # count matching lines
```

## Searching and Text Processing: grep

`grep` (global regular expression print) searches text for lines that match a pattern. It is one of the most used commands in Linux.

```
grep [options] pattern [file...]
```

### grep Options

| Option | Description |
|:---|:---|
| `-i` | Case insensitive search. |
| `-v` | Inverts the match, shows lines that do NOT match. |
| `-c` | Counts matching lines instead of printing them. |
| `-n` | Shows line numbers. |
| `-l` | Shows only the names of files that contain a match. |
| `-L` | Shows only the names of files that do NOT contain a match. |
| `-r` / `-R` | Searches directories recursively. |
| `-w` | Matches whole words only. |
| `-x` | Matches whole lines only. |
| `-o` | Prints only the matched part, not the whole line. |
| `-E` | Uses extended regular expressions, same as `egrep`. |
| `-F` | Treats the pattern as fixed text, not a regex. |
| `-A 3` | Shows 3 lines after each match. |
| `-B 3` | Shows 3 lines before each match. |
| `-C 3` | Shows 3 lines before and after each match. |
| `-q` | Quiet mode, only sets the exit code. Useful in scripts. |
| `-m 5` | Stops after 5 matches. |
| `--include="*.py"` | Limits a recursive search to matching files. |
| `--exclude-dir=.git` | Skips a directory during a recursive search. |
| `-P` | Enables Perl compatible regex, for example lookahead. |

### grep Examples

```
grep error /var/log/syslog            # lines containing error
grep -i error /var/log/syslog         # ignore case
grep -in "error" app.log              # case insensitive with line numbers
grep -v DEBUG app.log                 # everything except DEBUG lines
grep -c "404" access.log              # count matching lines
grep -rn "TODO" .                     # recursive search in current directory
grep -rl "api_key" /etc               # files that contain the word
grep -r --include="*.py" "import os" .
grep -w "cat" animals.txt             # whole word only, not catalog
grep -A 3 -B 1 "Exception" app.log    # context around matches
grep -E "error|fail|critical" app.log # extended regex with alternation
grep -o "[0-9]\{1,3\}\.[0-9]\{1,3\}" file.txt  # extract IP-like numbers
grep -q "ready" status.txt && echo "service ready"  # test in scripts
ps aux | grep nginx                   # find a running process
history | grep ssh                    # find an old command
```

### Regex Basics

| Pattern | Meaning |
|:---|:---|
| `^text` | Start of line. |
| `text$` | End of line. |
| `.` | Any single character. |
| `*` | Zero or more of the previous character. |
| `\+` | One or more (with `-E`, just `+`). |
| `[abc]` | Any one of a, b, or c. |
| `[^abc]` | Any character except a, b, or c. |
| `[0-9]` | Any digit. |
| `\(abc\)` | Group (with `-E`, `(abc)`). |
| `a\|b` | a or b (with `-E`, `a\|b`). |
| `\bword\b` | Word boundary. |

Tip: when a pattern contains shell special characters, wrap it in double quotes: `grep "^#" file` or `grep "a|b" file`.

### find, the Companion of grep

`find` searches the filesystem for files and directories by name, size, time, owner, and permissions.

```
find [path] [conditions] [action]
```

| Example | Description |
|:---|:---|
| `find . -name "*.py"` | Files ending in .py below the current directory. |
| `find / -iname "*.jpg"` | Case insensitive search by name. |
| `find . -type f` | Only files. |
| `find . -type d` | Only directories. |
| `find . -type f -size +100M` | Files larger than 100 MB. |
| `find . -mtime -7` | Files modified in the last 7 days. |
| `find . -mtime +30 -name "*.log"` | Log files older than 30 days. |
| `find . -user alice` | Files owned by alice. |
| `find . -perm 644` | Files with exact permissions 644. |
| `find . -empty` | Empty files and directories. |
| `find . -maxdepth 2 -name "*.conf"` | Limit how deep the search goes. |
| `find . -name "*.tmp" -delete` | Delete all matching files. |
| `find . -name "*.log" -exec gzip {} \;` | Run a command on every match. |
| `find . -type f -print0 \| xargs -0 grep -l "TODO"` | Safe handling of names with spaces. |

Examples:

```
find /var/log -name "*.log" -mtime +30 -delete
find . -type f -name "*.sh" -exec chmod +x {} \;
find ~/Downloads -type f -size +1G -exec ls -lh {} \;
```

### sed, awk, and Other Text Tools

`sed` edits text in a stream, and `awk` processes columns and records. Together with `grep` they form the classic Linux text processing trio.

```
sed "s/old/new/" file.txt          # replace first match per line, print to screen
sed "s/old/new/g" file.txt         # replace all matches per line
sed -i "s/old/new/g" file.txt      # edit the file in place
sed -i.bak "s/old/new/g" file.txt  # in place with a .bak backup
sed -n "10,20p" file.txt           # print only lines 10 to 20
sed "/^#/d" config.conf            # delete comment lines
sed -E "s/([0-9]+)-([0-9]+)/\2-\1/" file.txt   # swap two numbers
awk "{print $1}" file.txt          # print the first column
awk -F: "{print $1}" /etc/passwd   # custom field separator
awk "{sum += $1} END {print sum}" numbers.txt    # sum a column
awk "NR==5" file.txt               # print line 5
awk "length($0) > 80" file.txt     # lines longer than 80 characters
```

| Command | Description |
|:---|:---|
| `cut -d: -f1 /etc/passwd` | Cuts a column by delimiter. |
| `sort file` | Sorts lines alphabetically. |
| `sort -n file` | Sorts numerically. |
| `sort -h file` | Sorts human readable sizes. |
| `sort -u file` | Sorts and removes duplicates. |
| `uniq file` | Removes adjacent duplicate lines, usually after `sort`. |
| `uniq -c file` | Counts occurrences of each line. |
| `tr "a-z" "A-Z" < file` | Translates characters to uppercase. |
| `tr -d "\r" < file` | Deletes carriage returns. |
| `tee file` | Writes to a file and to the screen at the same time. |
| `diff a.txt b.txt` | Shows differences between two files. |
| `xargs` | Builds command lines from input. |

Classic pipeline example, the top 5 most frequent IP addresses in a log:

```
awk "{print $1}" access.log | sort | uniq -c | sort -rn | head -5
```

## Permissions and Ownership

Every file has an owner, a group, and permission bits for the owner, the group, and others.

```
-rwxr-xr-- 1 alice dev 4096 Oct 2 10:00 deploy.sh
|└┬┘└┬┘└┬┘
| |  |  └── others: r--  (read only)
| |  └───── group:  r-x  (read and execute)
| └──────── owner:  rwx  (read, write, execute)
└────────── type: - file, d directory, l symlink
```

| Symbol | Meaning on a file | Meaning on a directory |
|:---|:---|:---|
| `r` | Read the contents. | List the entries. |
| `w` | Modify the contents. | Create, delete, rename entries. |
| `x` | Run the file as a program. | Enter the directory with `cd`. |

### chmod

`chmod` (change mode) changes permissions. There are two ways: symbolic and octal.

Symbolic mode uses `u` (user), `g` (group), `o` (others), `a` (all), with `+`, `-`, `=` to add, remove, or set permissions.

```
chmod +x script.sh          # everyone can execute
chmod u+x script.sh         # only the owner can execute
chmod go-w file.txt         # remove write from group and others
chmod u=rw,g=r,o= file.txt  # set exact permissions
chmod -R g+r shared/        # recursive change
```

Octal mode numbers: read = 4, write = 2, execute = 1. Add them per group.

| Octal | Symbolic | Meaning |
|:---|:---|:---|
| `755` | `rwxr-xr-x` | Common for directories and executables. |
| `644` | `rw-r--r--` | Common for regular files. |
| `600` | `rw-------` | Private files such as SSH keys. |
| `700` | `rwx------` | Private directories and scripts. |
| `777` | `rwxrwxrwx` | Everyone full access, avoid on servers. |

```
chmod 755 deploy.sh
chmod 644 index.html
chmod 600 ~/.ssh/id_ed25519
chmod -R 755 public/
chmod --reference=file1 file2    # copy permissions from another file
```

Special bits: `setuid` (4), `setgid` (2), and `sticky` (1). Example: `chmod 1777 /tmp` keeps the sticky bit so users can only delete their own files.

### chown and chgrp

`chown` changes the owner, and `chgrp` changes the group. These normally require `sudo` when changing another user.

```
sudo chown alice file.txt            # change owner
sudo chown alice:dev file.txt        # change owner and group
sudo chown -R www-data:www-data /var/www/html
sudo chgrp dev project/
```

### umask

`umask` controls the default permissions of newly created files and directories.

```
umask          # show current mask, for example 0022
umask 027      # new files will not be accessible to others
```

## Superuser Access: sudo

`sudo` (superuser do) runs a single command with root privileges. It keeps a log of who ran what, and normally asks for your own password, not the root password.

```
sudo [options] command
```

| Option | Description |
|:---|:---|
| `sudo command` | Runs one command as root. |
| `sudo -i` | Opens an interactive root login shell. |
| `sudo -s` | Opens a root shell without a full login environment. |
| `sudo -u bob command` | Runs the command as user bob. |
| `sudo -l` | Lists what the current user may run with sudo. |
| `sudo -k` | Forgets the cached sudo password. |
| `sudo -v` | Refreshes the password timeout. |

Examples:

```
sudo apt update                   # update package lists as root
sudo systemctl restart nginx      # restart a service
sudo !!                           # rerun the previous command with sudo
sudo visudo                       # safely edit /etc/sudoers
sudo -u postgres psql             # run a command as another user
```

### Configuring sudo

Use `sudo visudo` to edit the sudoers file safely, because it checks syntax before saving. Extra rules belong in a file inside `/etc/sudoers.d/`.

```
# Allow user alice to run all commands
alice ALL=(ALL:ALL) ALL

# Allow the deploy group to restart nginx without a password
%deploy ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx
```

Security notes:

- Prefer `sudo` over logging in as root, so actions are logged and temporary.
- Never give `NOPASSWD: ALL` to normal users.
- Always edit with `visudo`. A broken sudoers file can lock you out of root access.
- If you lock yourself out, boot into recovery mode and fix `/etc/sudoers`.
## Package Management: apt

`apt` (Advanced Package Tool) installs, updates, and removes software on Debian, Ubuntu, Linux Mint, Pop!_OS, and related distributions. It reads package lists from repositories, resolves dependencies, and verifies signatures.

```
sudo apt [options] command [package...]
```

### Most Used apt Commands

| Command | Description |
|:---|:---|
| `sudo apt update` | Downloads the latest package index from repositories. Always run this first. |
| `sudo apt upgrade` | Installs available updates without removing packages. |
| `sudo apt full-upgrade` | Upgrades and removes or adds packages when required. |
| `sudo apt install nginx` | Installs a package with its dependencies. |
| `sudo apt install nginx git curl` | Installs several packages at once. |
| `sudo apt remove nginx` | Removes a package but keeps its configuration files. |
| `sudo apt purge nginx` | Removes the package and its configuration files. |
| `sudo apt autoremove` | Removes packages that are no longer needed. |
| `sudo apt autoclean` | Clears old package files from the cache. |
| `apt search editor` | Searches package names and descriptions. |
| `apt show nginx` | Shows version, size, dependencies, and description. |
| `apt list --installed` | Lists installed packages. |
| `apt list --upgradable` | Lists packages with available updates. |
| `apt policy nginx` | Shows the installed and candidate versions. |
| `sudo apt install --reinstall nginx` | Reinstalls a package. |
| `sudo apt-mark hold nginx` | Prevents a package from being upgraded. |
| `sudo apt-mark unhold nginx` | Allows upgrades again. |
| `sudo apt edit-sources` | Opens the repository list in an editor. |
| `sudo apt clean` | Deletes all cached .deb files. |

Examples:

```
sudo apt update && sudo apt upgrade -y      # the standard update routine
sudo apt install -y build-essential         # non interactive install
sudo apt install ./package.deb              # install a local deb file with dependencies
apt list --installed 2>/dev/null | grep python
sudo apt remove --auto-remove nginx         # remove with unused dependencies
```

### Related Tools

| Command | Description |
|:---|:---|
| `dpkg -i package.deb` | Installs a .deb file without resolving dependencies. |
| `dpkg -l` | Lists installed packages. |
| `dpkg -L nginx` | Lists files installed by a package. |
| `dpkg -S /usr/sbin/nginx` | Shows which package owns a file. |
| `sudo add-apt-repository ppa:user/ppa-name` | Adds a PPA repository (Ubuntu). |
| `sudo snap install code --classic` | Installs a snap package. |
| `flatpak install flathub app.id` | Installs a Flatpak application. |

### Other Distributions

| Distribution | Update | Install | Remove |
|:---|:---|:---|:---|
| Fedora / RHEL | `sudo dnf upgrade` | `sudo dnf install pkg` | `sudo dnf remove pkg` |
| Arch / Manjaro | `sudo pacman -Syu` | `sudo pacman -S pkg` | `sudo pacman -Rns pkg` |
| openSUSE | `sudo zypper up` | `sudo zypper in pkg` | `sudo zypper rm pkg` |
| Alpine | `sudo apk update` | `sudo apk add pkg` | `sudo apk del pkg` |

Tip: `apt` and `apt-get` are compatible. Use `apt` interactively for nicer output, and `apt-get` inside scripts for stable output.

## Remote Access: ssh

`ssh` (secure shell) opens an encrypted connection to another machine. It is used for remote administration, file transfer, tunnels, and Git over SSH.

```
ssh [options] user@host
```

### Connecting

```
ssh alice@192.168.1.50            # connect by IP
ssh alice@server.example.com      # connect by hostname
ssh -p 2222 alice@server          # custom port, the default is 22
ssh -i ~/.ssh/mykey alice@server  # use a specific private key
ssh alice@server "df -h"          # run one command and exit
ssh -v alice@server               # verbose output for debugging
```

### SSH Keys

Keys are safer than passwords and allow passwordless login.

```
ssh-keygen -t ed25519 -C "alice@laptop"   # generate a key pair
ssh-keygen -t rsa -b 4096                 # RSA alternative
ls -l ~/.ssh                              # id_ed25519 (private), id_ed25519.pub (public)
ssh-copy-id alice@server                  # copy the public key to the server
ssh alice@server                          # now logs in without a password
```

Rules: never share the private key, never commit it to Git, and keep permissions tight:

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

### SSH Config File

Create `~/.ssh/config` to save connection settings:

```
Host prod
    HostName 203.0.113.10
    User deploy
    Port 2222
    IdentityFile ~/.ssh/prod_ed25519
    ServerAliveInterval 60

Host staging
    HostName staging.example.com
    User deploy
```

Now `ssh prod` connects with all settings applied.

### Copying Files over SSH

| Command | Description |
|:---|:---|
| `scp file.txt alice@server:/home/alice/` | Copies a file to a remote host. |
| `scp alice@server:/var/log/app.log .` | Copies a remote file to the current directory. |
| `scp -r project/ alice@server:/var/www/` | Copies a directory recursively. |
| `scp -P 2222 file.txt alice@server:~/` | Uses a custom port (capital P for scp). |
| `rsync -avz project/ alice@server:/var/www/project/` | Fast, resumable sync with compression. |
| `rsync -avz --delete src/ server:/backup/src/` | Mirror a directory, deleting removed files. |
| `sftp alice@server` | Interactive file transfer session. |

### SSH Tunnels and Advanced Use

```
ssh -L 8080:localhost:80 alice@server   # local port 8080 forwards to remote port 80
ssh -R 9000:localhost:3000 alice@server # remote port 9000 forwards to your local 3000
ssh -D 1080 alice@server                # SOCKS proxy on port 1080
ssh -J jumpuser@jumpbox alice@internal  # connect through a jump host
ssh -N -f -L 5432:db.internal:5432 alice@server  # background tunnel only
autossh -M 0 -L 8080:localhost:80 alice@server   # keep a tunnel alive
```

Best practices:

- Disable password login on servers after keys work: set `PasswordAuthentication no` in `/etc/ssh/sshd_config` and restart sshd.
- Change the default port and use `fail2ban` to reduce automated attacks.
- Use `ProxyJump` instead of exposing internal machines directly.
- Consider `sshfs alice@server:/var/www ~/mnt/www` to mount a remote directory locally.

### Basic Networking Commands

| Command | Description |
|:---|:---|
| `ip a` | Shows network interfaces and IP addresses. |
| `ip r` | Shows the routing table. |
| `ping -c 4 8.8.8.8` | Tests connectivity. |
| `curl -I https://example.com` | Fetches response headers. |
| `curl -O https://example.com/file.zip` | Downloads a file. |
| `wget -c https://example.com/big.iso` | Downloads with resume support. |
| `ss -tulpn` | Shows listening ports and the owning processes. |
| `dig example.com` | Queries DNS records. |
| `traceroute example.com` | Shows the network path to a host. |
| `sudo ufw allow 22/tcp` | Opens a port in the UFW firewall. |

## Version Control: git

`git` tracks changes in files, supports branching and merging, and syncs work with remote repositories such as GitHub, GitLab, and Bitbucket.

### Concepts

| Term | Meaning |
|:---|:---|
| Working tree | The files you edit on disk. |
| Staging area | Changes selected for the next commit. |
| Commit | A saved snapshot with a message and hash. |
| Branch | A movable pointer to a line of commits. |
| Remote | A hosted copy of the repository, usually named origin. |

### First Time Setup

```
git config --global user.name "Alice Cruz"
git config --global user.email "alice@example.com"
git config --global init.defaultBranch main
git config --global core.editor "nano"
git config --global pull.rebase true
git config --global alias.st "status -sb"     # custom alias
git config --list                              # verify settings
```

### Starting a Repository

```
git init                          # start tracking a new project
git clone https://github.com/user/repo.git
git clone --depth 1 https://github.com/user/repo.git   # shallow clone
git clone -b develop https://github.com/user/repo.git  # clone a branch
```

### Daily Workflow

```
git status                        # what changed
git add file.txt                  # stage one file
git add .                         # stage everything in the current directory
git add -p                        # stage changes hunk by hunk
git commit -m "Add login form"    # save a snapshot
git commit --amend                # edit the last commit
git diff                          # unstaged changes
git diff --staged                 # staged changes
git restore file.txt              # discard unstaged changes
git restore --staged file.txt     # unstage but keep changes
git rm --cached secrets.env       # stop tracking a file, keep it on disk
```

### Branches and Merging

```
git branch                        # list local branches
git branch -a                     # include remote branches
git switch -c feature/login       # create and switch to a branch
git switch main                   # switch back
git checkout -b hotfix            # older syntax for the same thing
git merge feature/login           # merge into the current branch
git merge --no-ff feature/login   # always create a merge commit
git branch -d feature/login       # delete a merged branch
git branch -D feature/login       # force delete
git rebase main                   # replay commits on top of main
git rebase -i HEAD~3              # squash or edit the last three commits
git cherry-pick a1b2c3d           # apply one commit from another branch
```

Resolving a merge conflict:

```
git status                        # lists conflicted files
# edit the files and remove the conflict markers
git add resolved-file.txt
git commit
```

### Remotes and Collaboration

```
git remote add origin https://github.com/user/repo.git
git remote -v                     # show remote URLs
git push -u origin main           # first push, sets upstream
git push                          # later pushes
git push origin feature/login
git fetch                         # download changes without merging
git pull                          # fetch and merge (or rebase)
git pull --rebase                 # keep history linear
git push --tags
```

### History and Inspection

```
git log                           # full history
git log --oneline --graph --all --decorate   # compact visual history
git log -p file.txt               # changes to one file
git log --stat                    # files changed per commit
git show a1b2c3d                  # details of one commit
git blame file.txt                # who last changed each line
git shortlog -sn                  # commits per author
```

### Undoing Changes

| Command | Effect |
|:---|:---|
| `git restore file.txt` | Discards unstaged changes in a file. |
| `git reset --soft HEAD~1` | Undoes the last commit, keeps changes staged. |
| `git reset HEAD~1` | Undoes the last commit, keeps changes unstaged. |
| `git reset --hard HEAD~1` | Undoes the last commit and discards changes. |
| `git revert a1b2c3d` | Creates a new commit that reverses an old commit. |
| `git reflog` | Shows every recent HEAD movement, the safety net. |
| `git clean -fd` | Deletes untracked files and directories. |

### Stash and Ignore

```
git stash                         # save work temporarily
git stash list                    # show stashes
git stash pop                     # restore and remove the latest stash
git stash apply stash@{2}         # restore a specific stash and keep it
git stash drop                    # delete a stash
```

A `.gitignore` file lists what Git should not track:

```
.venv/
__pycache__/
*.log
.env
node_modules/
!.gitkeep
```

Best practices: commit small and often, write clear messages in the imperative mood, pull before pushing, never commit secrets or keys, and use branches plus pull requests for team work.
## Process Management

### Viewing Processes

| Command | Description |
|:---|:---|
| `ps aux` | Shows every process with user, CPU, and memory. |
| `ps -ef` | Full format listing with parent process IDs. |
| `ps aux --sort=-%mem \| head` | Top memory users. |
| `top` | Live process viewer, press `q` to quit, `M` sorts by memory, `P` by CPU. |
| `htop` | Friendlier interactive viewer, install with `sudo apt install htop`. |
| `pgrep -af nginx` | Finds process IDs by name with full command line. |
| `pidof nginx` | Shows the PID of a program. |
| `free -h` | Shows RAM and swap usage. |
| `uptime` | Shows load average for 1, 5, and 15 minutes. |
| `watch -n 2 df -h` | Repeats a command every 2 seconds. |

### Controlling Processes

| Command | Description |
|:---|:---|
| `kill 1234` | Sends SIGTERM (15), a polite request to stop. |
| `kill -9 1234` | Sends SIGKILL, forces the process to stop immediately. |
| `killall nginx` | Kills all processes with that name. |
| `pkill -f "python app.py"` | Kills processes matching the full command line. |
| `Ctrl+Z` | Suspends the foreground process. |
| `bg` | Resumes the suspended job in the background. |
| `fg` | Brings the most recent background job to the foreground. |
| `jobs` | Lists jobs of the current shell. |
| `command &` | Runs a command in the background. |
| `nohup command &` | Keeps a command running after you log out. |
| `disown` | Detaches a job so the shell will not kill it on exit. |

### systemd Services

```
sudo systemctl start nginx        # start now
sudo systemctl stop nginx         # stop now
sudo systemctl restart nginx      # restart
sudo systemctl reload nginx       # reload config without a full restart
sudo systemctl status nginx       # current state and recent logs
sudo systemctl enable nginx       # start automatically at boot
sudo systemctl disable nginx      # do not start at boot
systemctl list-units --type=service --state=running
journalctl -u nginx -f            # follow service logs
journalctl -u nginx --since "1 hour ago"
sudo systemctl daemon-reload      # after editing a unit file
```

## Disks and Storage

| Command | Description |
|:---|:---|
| `df -h` | Shows free space per mounted filesystem in readable units. |
| `df -i` | Shows inode usage, useful when small files fill a disk. |
| `du -sh folder/` | Shows the total size of a folder. |
| `du -h --max-depth=1 .` | Shows the size of each subfolder one level deep. |
| `du -ah . \| sort -h \| tail -10` | Ten largest files and folders. |
| `lsblk` | Lists block devices and their mount points. |
| `blkid` | Shows UUIDs and filesystem types. |
| `sudo mount /dev/sdb1 /mnt/usb` | Mounts a device. |
| `sudo umount /mnt/usb` | Unmounts a device. |
| `cat /etc/fstab` | Shows filesystems mounted at boot. |
| `ncdu` | Interactive disk usage browser, install with `sudo apt install ncdu`. |

Caution: tools such as `fdisk`, `parted`, `mkfs`, and `dd` can erase disks. Always verify the device name with `lsblk` before writing.

## Archives and Compression

| Command | Description |
|:---|:---|
| `tar -cvf backup.tar folder/` | Creates an uncompressed tar archive. |
| `tar -czvf backup.tar.gz folder/` | Creates a gzip compressed archive. |
| `tar -xzvf backup.tar.gz` | Extracts a gzip archive. |
| `tar -xzvf backup.tar.gz -C /opt/` | Extracts into a specific directory. |
| `tar -tzvf backup.tar.gz` | Lists contents without extracting. |
| `tar -czvf app.tar.gz --exclude="node_modules" app/` | Excludes a folder. |
| `gzip file.log` / `gunzip file.log.gz` | Compresses or decompresses one file. |
| `zip -r site.zip site/` | Creates a zip archive. |
| `unzip site.zip -d /var/www/` | Extracts a zip into a directory. |
| `unzip -l site.zip` | Lists zip contents. |
| `xz -9 file` / `unxz file.xz` | Strong compression for large files. |

Common backup pipeline: `tar -czf /backup/www-$(date +%F).tar.gz /var/www/html`.

## Users and Groups

| Command | Description |
|:---|:---|
| `whoami` | Current user name. |
| `id` | Current user ID, group ID, and all groups. |
| `who` / `w` | Who is logged in and what they are doing. |
| `last` | Login history. |
| `sudo adduser alice` | Creates a user with a home directory (friendly wrapper). |
| `sudo useradd -m -s /bin/bash alice` | Creates a user manually. |
| `sudo passwd alice` | Sets or changes a password. |
| `sudo usermod -aG sudo alice` | Adds alice to the sudo group. |
| `sudo usermod -aG docker $USER` | Adds yourself to the docker group (re-login needed). |
| `groups alice` | Lists groups of a user. |
| `sudo deluser alice` | Deletes a user. |
| `sudo groupadd dev` | Creates a group. |

Key files: `/etc/passwd` (accounts), `/etc/group` (groups), `/etc/shadow` (hashed passwords, readable only by root), `/etc/skel` (default home files).

## Environment and Shell Customization

```
printenv                        # show all environment variables
echo $HOME                      # print one variable
echo $PATH                      # where the shell looks for programs
export API_KEY="abc123"         # set a variable for this shell and child processes
unset API_KEY                   # remove a variable
export PATH="$HOME/.local/bin:$PATH"   # add a directory to PATH
alias ll="ls -lah"              # create a shortcut
alias gs="git status -sb"
unalias ll                      # remove an alias
source ~/.bashrc                # reload shell configuration
history                         # show command history
history | grep ssh              # search history
```

Startup files:

| File | Purpose |
|:---|:---|
| `~/.bashrc` | Runs for every interactive bash shell. Put aliases and functions here. |
| `~/.bash_profile` / `~/.profile` | Runs at login. Often sources `~/.bashrc`. |
| `/etc/bash.bashrc`, `/etc/profile` | System wide defaults. |
| `~/.zshrc` | Configuration for zsh users. |

Custom prompt example for `~/.bashrc`:

```
PS1="\u@\h:\w\$ "       # user@host:directory$
PS1="\[\e[32m\]\u@\h\[\e[0m\]:\w\$ "   # green user and host
```

## Redirection, Pipes, and Wildcards

Every command has three streams: standard input (0), standard output (1), and standard error (2).

| Operator | Meaning |
|:---|:---|
| `>` | Redirect stdout, overwriting the file. |
| `>>` | Redirect stdout, appending to the file. |
| `2>` | Redirect stderr only. |
| `2>>` | Append stderr only. |
| `&>` | Redirect stdout and stderr together. |
| `> file 2>&1` | Portable way to capture both streams. |
| `2>/dev/null` | Discard error messages. |
| `<` | Feed a file as stdin. |
| `<<` | Heredoc, inline multi line input. |
| `[cmd1] [pipe] [cmd2]` | Sends stdout of one command to stdin of the next. |
| `tee` | Writes to a file and the screen at once. |
| `&&` | Runs the next command only if the first succeeded. |
| `\|\|` | Runs the next command only if the first failed. |
| `;` | Runs commands in sequence regardless of success. |
| `&` | Runs a command in the background. |
| `$?` | Exit code of the last command, 0 means success. |

```
ls -la > listing.txt              # save output to a file
echo "new line" >> notes.txt     # append
command 2> errors.log             # capture only errors
command &> all.log                # capture everything
grep ERROR app.log                | sort | uniq -c     # pipeline
ls | tee listing.txt | wc -l      # save and count at the same time
apt update && apt upgrade -y      # run the second only on success
mkdir /tmp/x || echo "failed"     # handle failure
```

### Wildcards and Quoting

| Pattern | Matches |
|:---|:---|
| `*` | Any number of characters. |
| `?` | Exactly one character. |
| `[abc]` | One character from the set. |
| `[0-9]` | One digit. |
| `{a,b,c}` | Brace expansion into three words. |
| `~` | Your home directory. |

```
ls *.conf                         # all files ending in .conf
rm report-?.pdf                   # report-1.pdf but not report-10.pdf
cp file{1,2,3}.txt backup/        # expands to file1.txt file2.txt file3.txt
mkdir -p app/{src,tests,docs}     # three directories created
```

Quoting rules:

| Style | Behavior |
|:---|:---|
| `"double quotes"` | Expands variables and command substitution, protects spaces and wildcards. |
| `"single quotes"` | Literal text, nothing is expanded. |
| `\` | Escapes the next character, for example `my\ file.txt`. |

Important: globs such as `*` are expanded by the shell, while regex such as `.*` is interpreted by programs like grep.

## Scheduling Tasks

`cron` runs commands on a schedule. Edit your schedule with `crontab -e`.

```
* * * * * command
│ │ │ │ └── day of week (0-7, Sunday is 0 or 7)
│ │ │ └──── month (1-12)
│ │ └────── day of month (1-31)
│ └──────── hour (0-23)
└────────── minute (0-59)
```

| Schedule | Meaning |
|:---|:---|
| `*/5 * * * *` | Every 5 minutes. |
| `0 * * * *` | Every hour on the hour. |
| `0 3 * * *` | Every day at 03:00. |
| `0 3 * * 0` | Every Sunday at 03:00. |
| `@reboot` | Once at system startup. |
| `@daily` / `@weekly` / `@monthly` | Convenient shortcuts. |

```
crontab -e                        # edit your cron jobs
crontab -l                        # list your cron jobs
crontab -r                        # remove all your cron jobs

# backup every day at 2 AM and log the result
0 2 * * * /usr/bin/tar -czf /backup/www-$(date +\%F).tar.gz /var/www/html >> /var/log/backup.log 2>&1
```

Notes: cron uses a minimal environment, so use full paths and escape percent signs. Output is emailed unless redirected. Modern systems also offer `systemd` timers as a cron alternative. `at 15:30` schedules a one time job.

## Bash Scripting: Basic to Master

### A First Script

```
#!/usr/bin/env bash
# hello.sh - first script
set -euo pipefail        # exit on error, undefined variable, and failed pipeline

name="${1:-world}"       # first argument, with a default
echo "Hello, $name!"
```

```
chmod +x hello.sh
./hello.sh Alice
```

### Variables and Arguments

| Expression | Meaning |
|:---|:---|
| `$1`, `$2` | First and second argument. |
| `$0` | Script name. |
| `$#` | Number of arguments. |
| `"$@"` | All arguments as separate words, always quote it. |
| `$?` | Exit code of the previous command. |
| `$$` | PID of the current shell. |
| `${var:-default}` | Use a default when var is empty. |
| `${var:?message}` | Stop with an error when var is empty. |
| `$(command)` | Command substitution, captures output. |
| `$((2 + 3))` | Arithmetic expansion. |

```
count=$(ls | wc -l)
total=$((price * quantity))
echo "There are $count files"
```

### Conditionals

```
if [[ -f "$file" ]]; then
    echo "file exists"
elif [[ -d "$file" ]]; then
    echo "it is a directory"
else
    echo "not found"
fi

case "$1" in
    start) echo "starting..." ;;
    stop)  echo "stopping..." ;;
    *)     echo "usage: $0 {start|stop}" ; exit 1 ;;
esac
```

| Test | True when |
|:---|:---|
| `-f file` | It is a regular file. |
| `-d dir` | It is a directory. |
| `-e path` | It exists. |
| `-r` / `-w` / `-x` | Readable / writable / executable. |
| `-z "$s"` / `-n "$s"` | String is empty / not empty. |
| `"$a" = "$b"` | Strings are equal. |
| `$a -eq $b` | Integers are equal (`-ne`, `-lt`, `-gt`, `-le`, `-ge`). |

### Loops

```
for file in *.log; do
    echo "compressing $file"
    gzip "$file"
done

for i in {1..5}; do echo "run $i"; done

while read -r line; do
    echo "line: $line"
done < input.txt

until ping -c 1 -W 1 server > /dev/null 2>&1; do
    echo "waiting for server..."
    sleep 2
done
```

### Functions, Traps, and Robustness

```
#!/usr/bin/env bash
set -euo pipefail

log() {
    echo "[$(date +%H:%M:%S)] $*"
}

cleanup() {
    rm -f "$tmpfile"
}
trap cleanup EXIT INT TERM

tmpfile=$(mktemp)
log "starting backup"
tar -czf /backup/app-$(date +%F).tar.gz /var/www/app
log "done"
```

Master level practices:

- Quote every variable: `"$var"`, `"$@"`, `"$(command)"`.
- Start scripts with `set -euo pipefail` and check with `shellcheck script.sh`.
- Use `[[ ]]` for tests in bash, and `(( ))` for arithmetic.
- Use `mktemp` for temporary files and `trap` for cleanup.
- Log with timestamps and exit codes, and return meaningful exit codes.
- Prefer `rsync` and `tar` for backups, and test restore procedures.
- Keep scripts idempotent so running them twice is safe.

## Master Level Tools and One-Liners

| Tool | Use |
|:---|:---|
| `strace -p PID` | Traces system calls of a process, great for debugging hangs. |
| `lsof -i :8080` | Shows which process holds a port. |
| `lsof +D /var/log` | Shows processes using files in a directory. |
| `tcpdump -i any port 443` | Captures network traffic for analysis. |
| `tmux` / `screen` | Persistent terminal sessions that survive disconnects. |
| `rsync -avz --progress src/ dst/` | Efficient sync and backup. |
| `jq ".items[].name" data.json` | Parses and filters JSON. |
| `parallel` | Runs jobs in parallel with controlled concurrency. |
| `dmesg -T` | Kernel messages with timestamps, useful for hardware issues. |
| `sysctl -a` | Shows and tunes kernel parameters. |
| `ulimit -a` | Shows resource limits for the shell. |
| `time command` | Measures how long a command takes. |
| `watch -n 1 command` | Repeats a command and shows live changes. |

One-liners worth memorizing:

```
# top 10 largest files under the current directory
find . -type f -exec du -h {} + | sort -rh | head -10

# count lines of code by file type
find . -name "*.py" -exec wc -l {} + | tail -1

# find and replace in all files of a project
grep -rl "old.domain" . | xargs sed -i "s/old.domain/new.domain/g"

# watch a log and highlight errors
tail -f app.log | grep --color=always -E "ERROR|WARN|$"

# check which service listens on a port
sudo ss -tulpn | grep :443

# compare two directory trees
diff -r dir1/ dir2/

# show disk usage of the 10 largest directories
du -h --max-depth=1 /var | sort -rh | head -10

# monitor a command every 2 seconds
watch -n 2 "df -h; free -h"

# kill every process matching a pattern
pkill -f "celery worker"

# copy a whole server directory over SSH with compression
rsync -avz --delete /var/www/ deploy@server:/var/www/
```

## Cheatsheet

### Navigation

| Command | Description |
|:---|:---|
| `pwd` | Show the current directory. |
| `ls -lah` | List all files with sizes and permissions. |
| `cd /path` | Go to a directory. |
| `cd -` | Go to the previous directory. |
| `cd ..` | Go up one level. |

### Files and Directories

| Command | Description |
|:---|:---|
| `mkdir -p a/b/c` | Create nested directories. |
| `cp -r src dst` | Copy a directory. |
| `mv old new` | Move or rename. |
| `rm -rf dir` | Delete a directory and contents, use with care. |
| `ln -s target link` | Create a symbolic link. |
| `touch file` | Create an empty file. |
| `file file` | Detect the file type. |

### Viewing and Searching

| Command | Description |
|:---|:---|
| `cat file` | Print a file. |
| `less file` | Scroll through a file. |
| `head -n 20 file` | First 20 lines. |
| `tail -f file` | Follow a growing file. |
| `grep -rin "text" .` | Recursive case insensitive search with line numbers. |
| `grep -rl "text" .` | Files containing text. |
| `find . -name "*.log"` | Find files by name. |
| `sed -i "s/a/b/g" file` | Replace text in place. |
| `awk -F: "{print $1}" file` | Print the first field. |

### Permissions and Privileges

| Command | Description |
|:---|:---|
| `chmod 755 script.sh` | Set permissions in octal. |
| `chmod +x script.sh` | Make a file executable. |
| `chown user:group file` | Change owner and group. |
| `sudo command` | Run a command as root. |
| `sudo -i` | Open a root shell. |
| `sudo visudo` | Edit sudo rules safely. |

### Packages and Services

| Command | Description |
|:---|:---|
| `sudo apt update` | Refresh package lists. |
| `sudo apt upgrade` | Install updates. |
| `sudo apt install pkg` | Install software. |
| `sudo apt remove pkg` | Remove software. |
| `apt search pkg` | Search for a package. |
| `sudo systemctl restart svc` | Restart a service. |
| `journalctl -u svc -f` | Follow service logs. |

### SSH and Network

| Command | Description |
|:---|:---|
| `ssh user@host` | Connect to a remote machine. |
| `ssh-keygen -t ed25519` | Create an SSH key pair. |
| `ssh-copy-id user@host` | Install your key on a server. |
| `scp file user@host:~/` | Copy a file over SSH. |
| `rsync -avz src/ user@host:dst/` | Sync directories over SSH. |
| `curl -I https://site` | Check HTTP headers. |
| `ss -tulpn` | List listening ports. |
| `ip a` | Show IP addresses. |

### Git

| Command | Description |
|:---|:---|
| `git init` | Start a repository. |
| `git clone url` | Copy a remote repository. |
| `git status` | Show changed files. |
| `git add .` | Stage changes. |
| `git commit -m "msg"` | Save a snapshot. |
| `git push -u origin main` | Upload the first time. |
| `git pull --rebase` | Get remote changes on top. |
| `git switch -c feature` | Create a branch. |
| `git log --oneline --graph --all` | Visual history. |
| `git stash` | Shelve work temporarily. |

### Processes and Disk

| Command | Description |
|:---|:---|
| `ps aux` | List processes. |
| `top` / `htop` | Live process monitor. |
| `kill -9 PID` | Force kill a process. |
| `df -h` | Free disk space. |
| `du -sh dir` | Size of a directory. |
| `tar -czvf a.tar.gz dir/` | Create a compressed archive. |
| `tar -xzvf a.tar.gz` | Extract an archive. |

## Best Practices

- Read a command in the manual before using destructive options, especially `rm -rf`, `dd`, `mkfs`, and `chmod -R`.
- Make backups before large changes, and test restores, not just backups.
- Use `sudo` for single commands, keep root logins disabled, and never share private keys.
- Keep the system updated with `sudo apt update && sudo apt upgrade`.
- Quote variables in scripts, use `set -euo pipefail`, and check scripts with `shellcheck`.
- Prefer SSH keys over passwords, and disable password authentication on servers.
- Commit early and often, write meaningful messages, and never commit secrets.
- Test risky commands in a virtual machine, container, or snapshot first.
- Learn the pipeline mindset: small commands combined with pipes solve large problems.
- Do not paste commands from the internet into a root shell without understanding them.

