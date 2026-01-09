# Operating System Basics Cheatsheet

| Term                  | Definition                                                                     | Example / Notes                              |
| --------------------- | ------------------------------------------------------------------------------ | -------------------------------------------- |
| OS (Operating System) | Software that manages hardware and software resources, provides user interface | Linux, Windows, macOS                        |
| Drivers               | Software that allows OS to communicate with hardware devices                   | Printer driver, GPU driver                   |
| Kernel                | Core component of OS that manages hardware and system resources                | Linux kernel, Windows NT kernel              |
| Libraries             | Precompiled code that programs can use to perform common tasks                 | libc (C standard library), OpenSSL           |
| Binaries              | Executable files compiled from source code                                     | /bin/ls, /usr/bin/python                     |
| Linux Flavours        | Different distributions of Linux with varying features                         | Ubuntu, CentOS, Debian, Fedora, Arch Linux   |
| Desktop OS            | OS designed for personal computers with GUI                                    | Windows 10, Ubuntu Desktop, macOS            |
| Server OS             | OS designed for servers, optimized for network services and stability          | CentOS Server, Ubuntu Server, Windows Server |



# Linux Directory and User/Group Cheat Sheet

## Directory Management

| **Scenario / Pattern**      | **Create Directory (mkdir)**             | **Delete Directory (rmdir / rm)**                     |
| --------------------------- | ---------------------------------------- | ----------------------------------------------------- |
| Single directory            | `mkdir directoryname`                    | `rmdir directoryname` *(if empty)*                    |
| Directory with space        | `mkdir "my folder"`                      | `rmdir "my folder"` *(if empty)*                      |
| Multiple directories        | `mkdir folder1 folder2 folder3`          | `rmdir folder1 folder2 folder3` *(if empty)*          |
| Nested directories          | `mkdir -p main/sub/one/two`              | `rmdir -p main/sub/one/two` *(if empty)*              |
| Multiple nested directories | `mkdir -p project/{bin,config,logs,tmp}` | `rmdir -p project/{bin,config,logs,tmp}` *(if empty)* |
| Non-empty directory         | `mkdir mydata` *(contains files)*        | `rm -r mydata` *(recursive, deletes all contents)*    |
| Non-empty directory safely  | `mkdir mydata` *(contains files)*        | `rm -ri mydata` *(prompts before deleting each file)* |


# Soft Link (Symbolic Link) vs Hard Link

| **Aspect**                        | **Soft Link (Symbolic Link)**                                       | **Hard Link**                                                                      |
| --------------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| **Precise Definition**            | A special file that stores the *path* to a target file or directory | An additional directory entry that points to the *same inode* as the original file |
| **Inode**                         | Has its **own inode**                                               | Shares the **same inode** with the original file                                   |
| **What is stored**                | Path (string) to the target                                         | Inode number of the file                                                           |
| **Create command**                | `ln -s target softlink_name`                                        | `ln target hardlink_name`                                                          |
| **Delete command**                | `rm softlink_name`                                                  | `rm hardlink_name`                                                                 |
| **If original file is deleted**   | Link becomes **broken (dangling)**                                  | File remains accessible until last hard link is deleted                            |
| **When data is actually deleted** | When target file is deleted                                         | When **link count reaches zero**                                                   |
| **Works across filesystems**      | ✅ Yes                                                               | ❌ No                                                                               |
| **Can link directories**          | ✅ Yes                                                               | ❌ No (blocked to prevent filesystem loops)                                         |
| **Link count (`ls -l`)**          | Does **not** affect target link count                               | Increases link count                                                               |
| **`ls -l` file type**             | Starts with `l`                                                     | Starts with `-` (regular file)                                                     |
| **Common use cases**              | Shortcuts, versioned configs, shared paths                          | Backups, preventing accidental deletion                                            |


**Golden rule:** Soft links care about **paths**. Hard links care about **inodes**.



# Linux File System Cheatsheet (Tabular Format)

| Directory | Purpose                                     | Examples / Notes                                                                                                          |
| --------- | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| /         | Root directory, top of hierarchy            | All other directories branch from here                                                                                    |
| /bin      | Essential binaries for all users            | ls, cp, mv                                                                                                                |
| /sbin     | System administration binaries              | ifconfig, iptables                                                                                                        |
| /usr      | User programs and utilities                 | /usr/bin (user commands), /usr/sbin (admin commands), /usr/lib (libraries)                                                |
| /etc      | Configuration files                         | /etc/passwd, /etc/ssh/sshd_config                                                                                         |
| /home     | User home directories                       | /home/alice, /home/bob                                                                                                    |
| /root     | Root user home directory                    | Only accessible by root                                                                                                   |
| /var      | Variable files that change frequently       | /var/log (logs), /var/tmp (temporary files), /var/spool (queues), /var/lib (application state), /var/cache (cached files) |
| /tmp      | Temporary files cleared on reboot           | /tmp/tmpfile.txt                                                                                                          |
| /dev      | Device files                                | /dev/sda (disk), /dev/tty (terminal)                                                                                      |
| /proc     | Virtual filesystem with system/process info | /proc/cpuinfo, /proc/meminfo                                                                                              |
| /sys      | Kernel and device info (sysfs)              | /sys/class, /sys/block                                                                                                    |
| /mnt      | Temporary mount points                      | /mnt/usb                                                                                                                  |
| /media    | Automatically mounted media                 | /media/cdrom, /media/usb                                                                                                  |
| /lib      | Essential shared libraries                  | /lib/libc.so.6                                                                                                            |
| /opt      | Optional third-party software               | /opt/google                                                                                                               |
| /boot     | Bootloader files and kernel                 | /boot/vmlinuz, /boot/grub                                                                                                 |
| /srv      | Service data                                | /srv/www (web server data)                                                                                                |

**Key Tips:**

* Permissions: `rwx` for owner/group/others
* Hidden files: start with `.`
* Paths: absolute (`/path`) vs relative (`path`)
* Useful commands: `ls -l`, `df -h`, `du -sh <dir>`

# File editors and search

| Command                   | Description                                                                               | Usage Example             | Shortcuts / Notes                                                                        |
| ------------------------- | ----------------------------------------------------------------------------------------- | ------------------------- | ---------------------------------------------------------------------------------------- |
| `vi`                      | A classic terminal-based text editor. Useful for editing configuration files and scripts. | `vi filename.txt`         | `i` - insert mode, `d` - delete line, `:wq` - save and exit, `:q!` - exit without saving |
| `nano`                    | A simpler terminal text editor, beginner-friendly.                                        | `nano filename.txt`       | `Ctrl+O` - save, `Ctrl+X` - exit, `Ctrl+K` - cut line, `Ctrl+U` - paste line             |
| `cat`                     | Displays file content in the terminal. Can also create files with redirection.            | `cat file.txt`            |                                                                                          |
| `cat > file.txt`          | N/A                                                                                       |                           |                                                                                          |
| `echo`                    | Prints text to the terminal or writes text to a file.                                     | `echo "Hello"`            |                                                                                          |
| `echo "Hello" > file.txt` | N/A                                                                                       |                           |                                                                                          |
| `grep`                    | Searches for patterns in files. Very useful for filtering text.                           | `grep 'pattern' file.txt` | `-i` ignore case, `-r` recursive, `-v` invert match                                      |


## User and Group Management

| **Scenario / Pattern**         | **Add / Modify (useradd / adduser / usermod / groupadd)**                                                                                                           | **Delete / Remove (userdel / gpasswd / groupdel)**       |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------| -------------------------------------------------------- |
| Add a new user                 | `adduser username` *(interactive, prompts for password and details)* OR `useradd username + passwd username` OR `useradd -m username (Creates home directory as well)  + passwd username` | `userdel username` *(add -r to remove home directory)*   |
| Add a new group                | `groupadd groupname`                                                                                                                                                 | `groupdel groupname`                                     |
| Add user to a specific group   | `usermod -aG groupname username`                                                                                                                                     | `gpasswd -d username groupname`                          |
| Add user to multiple groups    | `usermod -aG group1,group2,group3 username`                                                                                                                          | Run `gpasswd -d username group1` (repeat for each group) |
| Check users in a group         | `getent group groupname`                                                                                                                                             | N/A                                                      |
| Check groups a user belongs to | `groups username`                                                                                                                                                    | N/A                                                      |
| Check detailed user info       | `id username`                                                                                                                                                        | N/A                                                      |


# Linux File Ownership Cheat Sheet

| **Scenario / Pattern**                     | **Command / Example**                      | **Notes / Behavior**                                           |
| ------------------------------------------ | ------------------------------------------ | -------------------------------------------------------------- |
| **Change file owner**                      | `chown newowner filename`                  | Changes the owner of a file; keeps group unchanged             |
| **Change file owner and group**            | `chown newowner:newgroup filename`         | Changes both owner and group at the same time                  |
| **Change only the group**                  | `chgrp newgroup filename`                  | Alternative to changing only group using `chown`               |
| **Recursive ownership change**             | `chown -R newowner:newgroup directoryname` | Changes owner/group for directory and all files/subdirectories |
| **Change ownership of multiple files**     | `chown user1:user1 file1 file2 file3`      | Applies to all listed files                                    |
| **Change ownership using numeric UID/GID** | `chown 1001:1001 filename`                 | Useful when migrating users/groups between systems             |
| **Verbose output**                         | `chown -v newowner:newgroup filename`      | Shows which files were changed                                 |


# Linux File Permissions Cheat Sheet

## Notes

* Permissions are shown as `rwx` in `ls -l` output.
* Numeric method: add `r=4`, `w=2`, `x=1` per user/group/others.
* Symbolic method: `u` = owner, `g` = group, `o` = others, `a` = all.
* Avoid `777` in production — security risk.

| **Scenario / Pattern**                                  | **Absolute Method (Numeric)** | **Symbolic Method**             | **Explanation**                                 |
| ------------------------------------------------------- | ----------------------------- | ------------------------------- | ----------------------------------------------- |
| Owner full, group & others read                         | `chmod 744 file.txt`          | `chmod u=rwx,g=r,o=r file.txt`  | Owner can read/write/execute; group/others read |
| Owner full, group read/execute, others read             | `chmod 754 file.txt`          | `chmod u=rwx,g=rx,o=r file.txt` | Owner full, group read/execute, others read     |
| Owner read/write, group/others read                     | `chmod 644 file.txt`          | `chmod u=rw,g=r,o=r file.txt`   | Owner can read/write; others read only          |
| Owner read/write/execute, group read/write, others none | `chmod 760 file.txt`          | `chmod u=rwx,g=rw,o= file.txt`  | Owner full, group rw, others no permissions     |
| Add execute for owner only                              | N/A                           | `chmod u+x file.txt`            | Adds execute permission to owner only           |
| Remove write for group                                  | N/A                           | `chmod g-w file.txt`            | Removes write permission from group             |
| Give all permissions to all                             | `chmod 777 file.txt`          | `chmod a=rwx file.txt`          | Owner, group, others all have rwx               |


# SSH Key Generation Cheat Sheet

| **Scenario / Pattern**            | **Command**                                     | **Notes / Behavior**                                                |
| --------------------------------- | ----------------------------------------------- | ------------------------------------------------------------------- |
| Generate RSA 4096-bit key         | `ssh-keygen -t rsa -b 4096`                     | Prompts for file path and passphrase; default path: `~/.ssh/id_rsa` |
| Specify custom file name          | `ssh-keygen -t rsa -b 4096 -f /path/to/keyname` | Saves key to custom location                                        |
| Add passphrase for extra security | Enter passphrase when prompted                  | Protects private key; optional but recommended                      |
| View public key                   | `cat ~/.ssh/id_rsa.pub`                         | Use this to copy into `~/.ssh/authorized_keys` on a server          |
| Copy public key to remote server  | `ssh-copy-id user@remote_host`                  | Automates adding public key to server for passwordless login        |
| Test SSH key authentication       | `ssh user@remote_host`                          | Logs in without password if key is properly installed               |

## Notes

* Private key (`id_rsa`) **must be kept secret**.
* Public key (`id_rsa.pub`) can be shared with servers.
* RSA 4096-bit is stronger than default 2048-bit.
* Use `ssh-keygen -t ed25519` for a faster, modern key alternative.

**Cron Jobs Cheatsheet (Linux)**

| Feature                   | Description                                                      | Example                                                                        |
| ------------------------- | ---------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Definition                | Scheduled tasks in Linux/Unix that run automatically             | Backup scripts, log rotation                                                   |
| Basic Syntax              | `* * * * * command_to_run` (minute hour day month weekday)       | `0 2 * * * /path/to/script.sh` runs daily at 2:00 AM                           |
| Every 5 minutes           | `*/5 * * * *`                                                    | `*/5 * * * * /path/to/command`                                                 |
| Every half hour           | `0,30 * * * *`                                                   | `0,30 * * * * /path/to/command`                                                |
| Every 2 hours             | `0 */2 * * *`                                                    | `0 */2 * * * /path/to/command`                                                 |
| Specific day/time         | Run at 9 AM every Monday                                         | `0 9 * * 1 /path/to/command`                                                   |
| Using user/group          | Change ownership of scripts for cron                             | `chown alice:staff /path/to/script.sh` or `chown 1001:1001 /path/to/script.sh` |
| List user cron jobs       | Show all jobs for the current user                               | `crontab -l`                                                                   |
| Edit user cron jobs       | Open editor to modify cron jobs                                  | `crontab -e`                                                                   |
| Remove all user cron jobs | Delete all cron jobs for the user                                | `crontab -r`                                                                   |
| System-wide cron jobs     | Stored in `/etc/crontab` or `/etc/cron.d/`                       | Format includes user: `* * * * * username command_to_run`                      |
| Tips                      | - Ensure scripts have execute permissions (`chmod +x script.sh`) |                                                                                |

* When adding or modifying cron jobs, you can use [Crontab Guru](https://crontab.guru) to validate schedules.
* Use absolute paths in cron scripts
* Check logs: `/var/log/cron` or `journalctl -u cron` | N/A
