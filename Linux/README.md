# Linux Cheatsheet

## Table Of Contents

- [Linux Cheatsheet](#linux-cheatsheet)
  - [Table Of Contents](#table-of-contents)
  - [LPIC](#lpic)
    - [101.1 Determine and configure hardware settings](#1011-determine-and-configure-hardware-settings)
    - [101.2 Boot the System](#1012-boot-the-system)
    - [101.3 Change runlevels / boot targets and shutdown or reboot the system](#1013-change-runlevels--boot-targets-and-shutdown-or-reboot-the-system)
    - [102.1 Design hard disk layout](#1021-design-hard-disk-layout)
    - [102.2 Install a boot manager](#1022-install-a-boot-manager)
    - [102.3 Manage shared libraries](#1023-manage-shared-libraries)
    - [102.4 Use Debian package management](#1024-use-debian-package-management)
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

## LPIC

### 101.1 Determine and configure hardware settings
*Link to [source](https://linux1st.com/1011-determine-and-configure-hardware-settings.html)*

- **sysfs** is a pseudo file system provided by the Linux kernel
- **udev** (userspace `/dev`) is a device manager for the Linux kernel.
- **D-Bus** is a message bus system, a simple way for applications to talk to one another.
- **proc** is where the Kernel keeps its settings and properties.
  - `/proc/cpuinfo` or `meminfo`
- `lsusb`, `lspci`, `lsblk`, `lshw`, `lsmod`
- Remove and add modules:
  ```shell
  $ rmmod iwlwifi
  $ modprobe iwlwifi
  ```

### 101.2 Boot the System
*Link to [source](https://linux1st.com/1012-boot-the-system.html)*

- You can check `/sys/firmware/efi` or `/boot/efi` to see if you are using a **UEFI** system or not.
- `journalctl -k` to check Kernel logs or use `journalctl -b` to check for boot logs
```shell
$ systemctl list-units
$ systemctl list-units --type=target
$ systemctl cat graphical.target
$ systemctl stop sshd
$ systemctl start sshd
$ systemctl status sshd
$ systemctl enable sshd
```

### 101.3 Change runlevels / boot targets and shutdown or reboot the system
*Link to [source](https://linux1st.com/1013-change-runlevels-boot-targets-and-shutdown-or-reboot-the-system.html)*

1. `rescue`: Local file systems are mounted, there is no networking, and only root user (maintenance mode)
2. `emergency`: Only the root file system and in read-only mode, No networking and only root (maintenance mode)
3. `reboot`
4. `halt`: Stops all processes and halts CPU activities
5. `poweroff`: Like halt but also sends an ACPI shutdown signal (No lights!)
```shell  
$ systemctl isolate emergency
$ systemctl is-system-running
maintenance
```

**`shutdown` command:**

- Default is a 1-minute delay and then going to runlevel 1
- `-h` will halt the system
- `-r` will reboot the system
- Time is `hh:mm` or n (minutes) or now
- Whatever you add, will be broadcasted to logged-in users using the wall command
- If the command is running, `ctrl+c` or the `shutdown -c` will cancel it

**Notifying users:**
- `wall`: Sending wall messages to logged-in users
- `who -T` will show `mesg` status.

### 102.1 Design hard disk layout
*Link to [source](https://linux1st.com/1021-design-hard-disk-layout.html)*

| Directory | Description                                       |
| --------- | ------------------------------------------------- |
| bin       | Essential command binaries                        |
| boot      | Static files of the boot loader                   |
| dev       | Device files                                      |
| etc       | Host-specific system configuration                |
| home      | Home directory of the users                       |
| lib       | Essential shared libraries and kernel modules     |
| media     | Mount point for removable media                   |
| mnt       | Mount point for mounting a filesystem temporarily |
| opt       | Add-on application software packages              |
| root      | Home directory of the root user                   |
| sbin      | Essential system binaries                         |
| srv       | Data for services provided by this system         |
| tmp       | Temporary files, sometimes purged on each boot    |
| usr       | Secondary hierarchy                               |
| var       | Variable data (logs, ...)                         |

**Commands:**
```shell
$ fdisk /dev/sda
Command (m for help): p
```
```shell
$ sudo parted /dev/sda p
```
```shell
$ gparted # A graphical tool for managing disks and partitions.
```

**3 different swap design:**
- Debian 11: Uses a swap partition
- Ubuntu 22.04: Uses a swap file
- Fedora 36: uses zram In short, zram is a virtual disk on your RAM which can be used as swap space or be mounted anywhere you like; A common example is `\tmp`. Let's see.

### 102.2 Install a boot manager
*Link to [source](https://linux1st.com/1022-install-a-boot-manager.html)*


**GRUB2:** 

On BIOS systems it is installed on `/boot/grub/` or `/boot/grub2/` and under UEFI it goes in `/boot/efi/EFI/distro-name/` (say `/boot/efi/EFI/fedora/`). GRUB2's configuration file is called `grub.cfg`.

**Commands:**

```shell
grub-install /dev/sda # Install

update-grub
# or
grub2-mkconfig > /boot/grub2/grub.cfg
```
`update-grub`: Reads the configuration files from `/etc/grub.d/` and `/etc/default/grub/` and create the `grub.cfg`

### 102.3 Manage shared libraries
*Link to [source](https://linux1st.com/1023-manage-shared-libraries.html)*

Linux dynamic libraries have names like `libLIBNAME.so.VERSION` and are located at places like `/lib*/` and `/usr/lib*/`

The `ldd` command helps you find: 
- If a program is dynamically or statically linked
- What libraries a program needs

```shell
$ ldd /sbin/ldconfig
not a dynamic executable
```

**Configs:**

`/etc/ld.so.conf`

Run `ldconfig` to update the cache.

### 102.4 Use Debian package management
*Link to [source](https://linux1st.com/1024-use-debian-package-management.html)*

```shell
apt-get update  # Updating sources information
apt-get install tmux
apt-get install -s tmux # simulate
apt-get install --download-only tmux
apt-get remove tmux
apt-get autoremove # remove automatically installed dependencies
apt-get upgrade
apt-get dist-upgrade  # going to a new distribution
```

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

| File name     | Description                                                                                                    | Reference |
| ------------- | --------------------------------------------------------------------------------------------------------------- | --------- |
| `/etc/passwd` | Used to keep track of every registered user that has access to a system                                         | [Link][1] |
| `/tmp/fifo`   | Named pipe. It is a file that can be used as a communication channel between multi processes (`$ mkfifo pipe1`) | [Link][2] |
| `/etc/shadow` | The shadow file stores the hashed passwords                                                                     | [Link][3] |



[1]: https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/#:~:text=The%20%2Fetc%2Fpasswd%20is%20a,user%20IDs%20to%20user%20names.

[2]: https://www.baeldung.com/linux/anonymous-named-pipes#named-pipes

[3]: https://www.cyberciti.biz/faq/understanding-etcshadow-file/

[4]: https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/