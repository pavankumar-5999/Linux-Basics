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

