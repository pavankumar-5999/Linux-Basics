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

