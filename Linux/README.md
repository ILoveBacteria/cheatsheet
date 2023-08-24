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
    - [102.6 Linux as a virtualization guest](#1026-linux-as-a-virtualization-guest)
  - [Linux Commands](#linux-commands)
  - [Bash](#bash)

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

You can check `/sys/firmware/efi` or `/boot/efi` to see if you are using a **UEFI** system or not.

```shell
journalctl -k # to check Kernel logs
journalctl -b # check for boot logs
cat /var/log/dmesg  # show only the data during the boot.
cat /var/log/messages # Include init logs
```
**Which init is using**
```shell
$ which init
/sbin/init
$ readlink -f /sbin/init
/usr/lib/systemd/systemd
$ ps -p 1
PID TTY TIME     CMD
1   ?   00:00:06 systemd
$ pstree  # the hierarchy of processes
```
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

### 102.6 Linux as a virtualization guest

Link to [source](https://linux1st.com/1026-linux-as-a-virtualization-guest.html)

**Check CPU supports hypervisor**
`vmx` (for Intel CPUs) or `svm` (for AMD CPUs) in `/proc/cpuinfo` in flags. Based on the CPU you should have `kvm` or `kvm-amd` kernel modules loaded.
```shell
lsmod | grep -i kvm
sudo modprobe kvm
```

**Guest-specific configs**

After *cloning* we need to change these on each machine before booting them:
- Host Name
- NIC MAC Address
- NIC IP (If not using DHCP)
- Machine ID (delete the /etc/machine-id and /var/lib/dbus/machine-id and run dbus-uuidgen --ensure. These two files might be soft links to each other)
- Encryption Keys like SSH Fingerprints and PGP keys
- HDD UUIDs
- Any other UUIDs on the system

## Linux Commands

| Command          | Description                           |
| ---------------- | ------------------------------------- |
| `rev`            | Reverse lines of a file or files      |
| `seq`            | Print sequences of numbers            |
| `yes`            | Print a string until interrupted      |
| `date`           | Print or set the system date and time |
| `netstat -tulpn` | List all listening ports              |

## Bash

Read line by line from a file:
```shell
cat peptides.txt | while read line 
do
  # do something with $line here
done
```