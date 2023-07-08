# Linux Cheatsheet

## Table Of Contents

- [Linux Cheatsheet](#linux-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [Environment Variables](#environment-variables)
  - [Linux Commands](#linux-commands)
    - [`ls`](#ls)
    - [`cat`](#cat)
    - [`chmod`](#chmod)
    - [`which`](#which)
    - [`man`](#man)
    - [`cut`](#cut)
    - [`sort`](#sort)
    - [`grep`](#grep)
    - [`>`](#)
    - [`find`](#find)
    - [`xargs`](#xargs)
    - [`chown`](#chown)
    - [`sed`](#sed)
    - [`awk`](#awk)
    - [`wc`](#wc)
    - [`service`](#service)
    - [`systemctl`](#systemctl)
    - [`scp`](#scp)
    - [`nc`](#nc)
    - [`netstat`](#netstat)
    - [`file`](#file)
  - [Handwrite Notes](#handwrite-notes)
  - [Files](#files)

## Environment Variables

| Command                              | Description                                                           |
| ------------------------------------ | --------------------------------------------------------------------- |
| `$ echo $PATH`                       | View a variable                                                       |
| `$ export PATH=/the/file/path:$PATH` | Adding a directory to the start of PATH will mean it is checked first |
| `$ export PATH=$PATH:/the/file/path` | Adding a directory to the end of PATH means it will be checked last   |
| `$ export`                           | We can view a complete list of exported environment variables         |

**File and directories**

 `~/.profile`: To persist our changes for the current user, we add our export command to the end of `~/.profile`.
 
 `~/.zshenv`: To persist changes for zsh shell.

 `/etc/profile.d/`: We can add a new path for all users on a Unix-like system by creating a file ending in `.sh` in `/etc/profile.d/` and adding our export command to this file. For example we create this file `/etc/profile.d/http_proxy.sh`

## Linux Commands

### `ls`

- `$ ls -l`: List directory contents in long format
- `$ ls`: List directory contents
- `$ ls -a`: List all files including hidden files
- `$ ls -lh`: List directory contents in long format with human readable file sizes
- `$ ls -R`: List subdirectories recursively
- `$ ls -t`: Sort by time modified (most recently modified first)
- `$ ls -S`: Sort by file size (largest file first)
- `$ ls -r`: Reverse the order of the sort
- `$ ls -1`: List one file per line
- `$ ls -m`: Fill width with a comma separated list of entries

### `cat`

- `$ cat`: Concatenate files and print on the standard output
- `$ cat -n`: Number all output lines
- `$ cat -b`: Number nonempty output lines, overrides -n
- `$ cat -A`: Display nonprinting characters as ^ and M- notation, except for LFD and TAB
- `$ cat -E`: Display nonprinting characters as $ and M- notation, except for LFD and TAB

### `chmod`

- `$ chmod 777`: Give all permissions to all users
- `$ chmod +x`: Give execute permission to all users
- `$ chmod -x`: Remove execute permission to all users

### `which`

- `$ which <command>`: Locate a command

### `man`

- `$ man <command>`: Display the manual page of a command

### `cut`

- `$ cut -d <delimiter> -f <field>`: Cut out selected portions of each line of a file

### `sort`

- `$ sort -n`: Sort numerically
- `$ sort -r`: Reverse the result of comparisons
- `$ sort -h`: Compare human readable numbers (e.g., 2K 1G)

### `grep`

- `$ grep <pattern> <file>`: Search for a pattern in a file
- `$ grep -v <pattern> <file>`: Invert the match, to select non-matching lines
- `$ grep -c <pattern> <file>`: Count the number of matching lines
- `$ grep -E <pattern> <file>`: Interpret pattern as an extended regular expression (ERE)

### `>`

- `$ >`: Redirect output to a file

### `find`

- `$ find <path> -name <file_name>`: Find files by name
- `$ find <path> -type <file_type>`: Find files by type
- `$ find <path> -size <file_size>`: Find files by size
- `$ find <path> -name <file_name> 2>/dev/null`: Find files by name and redirect error to `/dev/null
- `$ find <path> -perm <file_permission>`: Find files by permission

### `xargs`

- `$ xargs <command>`: Execute a command with arguments from standard input
    - e.g. `$ find . -name "*.txt" | xargs rm`

### `chown`

- `$ chown <user>:<group> <file>`: Change the owner and group of a file

### `sed`

- `$ sed 's/unix/linux/2' <file>`: Replacing the 2th occurrence of a pattern in a line
- `$ sed 's/unix/linux/g' <file>`: Replacing all the occurrence of the pattern in a line

### `awk`

- `$ awk '{print $1}' <file>`: Print the first column of a file
- `$ awk '{print $1, $2}' <file>`: Print the first and second column of a file
- `$ awk "Begin{s=0} {s=s+$1} End{print s}" <file>`: Sum the first column of a file

### `wc`

- `$ wc <file>`: Count the number of words
- `$ wc -l <file>`: Count the number of lines in a file

### `service`

- `$ service <service_name> start`: Start a service
- `$ service <service_name> stop`: Stop a service
- `$ service <service_name> restart`: Restart a service
- `$ service <service_name> status`: Check the status of a service

### `systemctl`

- `$ systemctl start <service_name>`: Start a service
- `$ systemctl enable <service_name>`: Enable a service to start at boot

### `scp`

- `$ scp -r <file> <user>@<host>:<path>`: Copy a file to a remote host
    - `-r`: Copy a directory recursively
    - To be able to copy files, you must have at least read permissions on the source file and write permission on the target system.
    - Be careful when copying files that share the same name and location on both systems, scp will overwrite files without warning.

[Reference][4]

### `nc`

- `$ nc -l -p <port>`: Listen on a port
- `$ nc -lp 8080 -e /bin/bash`: Listen on a port and execute a command
- `$ nc -z <host>`: Check if a port is open
- `$ nc <host> <port> < <file>`: Send the content of a file to a port
- `$ nc <host> <port> > <file>`: Receive the content of a port and save it to a file

### `netstat`

- `$ netstat`: List all listening ports

### `file`

- `$ file <file>`: Determine file type and details
 
## Handwrite Notes

1. `$ echo moein | hexdump -e '8/1 "%02X " "\n"' | sed 's/ //g'`: Convert a string to hex
    - `8/1 "%02X"`: Print 8 bytes per line in hex format
2. Ubuntu is **Debian** based
3. Fedora is **Red Hat** based 

## Files

|File name|Descriprtion|Reference|
|---|---|---|
|`/etc/passwd`|Used to keep track of every registered user that has access to a system|[Link][1]|
|`/tmp/fifo`|Named pipe. It is a file that can be used as a communication channel between multi processes (`$ mkfifo pipe1`)|[Link][2]|
|`/etc/shadow`|The shadow file stores the hashed passwords|[Link][3]|



[1]: https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/#:~:text=The%20%2Fetc%2Fpasswd%20is%20a,user%20IDs%20to%20user%20names.

[2]: https://www.baeldung.com/linux/anonymous-named-pipes#named-pipes

[3]: https://www.cyberciti.biz/faq/understanding-etcshadow-file/

[4]: https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/