# Linux for DevOps — Notes Index

These notes expand the original `Linux_for_DevOps_Day_2_to_Last_Day_Commands.md` into shorter, topic-based study files. Use the original file as a quick command checklist and these files for detailed learning and practice.

## Topic notes

| File                                                                         | Command type                            | Main commands                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`01_Basics.md`](01_Basics.md)                                               | Linux and DevOps fundamentals           | Shell, kernel, filesystem, processes, SSH, cloud basics                                                                                                                                                                            |
| [`02_Files_and_Directories.md`](02_Files_and_Directories.md)                 | Filesystem/navigation commands          | `pwd`, `ls`, `cd`, `mkdir`, `touch`, `cp`, `mv`, `rm`, `rmdir`, `ln`, `clear`                                                                                                                                                      |
| [`03_Viewing_and_Editing.md`](03_Viewing_and_Editing.md)                     | File viewing and editing                | `cat`, `head`, `tail`, `less`, `more`, `echo`, `tee`, `vi`, `vim`                                                                                                                                                                  |
| [`04_Text_Processing.md`](04_Text_Processing.md)                             | Text/log processing                     | `grep`, `cut`, `sort`, `wc`, `awk`, `sed`, `diff`, pipes/redirection                                                                                                                                                               |
| [`05_Processes_and_Resources.md`](05_Processes_and_Resources.md)             | Processes and system monitoring         | `ps`, `top`, `kill`, `fuser`, `free`, `vmstat`, `df`, `du`, `nohup`                                                                                                                                                                |
| [`06_Users_Groups_and_Permissions.md`](06_Users_Groups_and_Permissions.md)   | Users, groups, and permissions          | `whoami`, `id`, `who`, `which`, `sudo`, `useradd`, `passwd`, `su`, `userdel`, `groupadd`, `gpasswd`, `groupdel`, `chmod`, `umask`, `chown`, `chgrp`, `history`                                                                     |
| [`07_Archives_and_Remote_Transfers.md`](07_Archives_and_Remote_Transfers.md) | Compression and remote access           | `zip`, `unzip`, `gzip`, `gunzip`, `tar`, `ssh`, `scp`, `rsync`                                                                                                                                                                     |
| [`08_Networking_and_HTTP.md`](08_Networking_and_HTTP.md)                     | Networking, DNS, HTTP, and firewall     | `ping`, `netstat`, `ifconfig`, `traceroute`, `tracepath`, `mtr`, `nslookup`, `telnet`, `hostname`, `ip`, `iwconfig`, `ss`, `dig`, `whois`, `nc`, `arp`, `ifplugstatus`, `curl`, `jq`, `wget`, `iptables`, `watch`, `nmap`, `route` |
| [`09_System_and_Packages.md`](09_System_and_Packages.md)                     | System information and package managers | `date`, `uname`, `uptime`, `who`, `shutdown`, `reboot`, `apt`, `dnf`, `pacman`, Portage                                                                                                                                            |
| [`10_Storage_and_LVM.md`](10_Storage_and_LVM.md)                             | Disks, filesystems, mounting, and LVM   | `lsblk`, `df`, `pvcreate`, `vgcreate`, `lvcreate`, `pvdisplay`, `vgdisplay`, `lvdisplay`, `mkfs.ext4`, `mkfs.xfs`, `mkdir`, `mount`, `umount`, `lvextend`                                                                          |

## Suggested learning order

1. Read [`01_Basics.md`](01_Basics.md) first if you have not completed the Day 1 fundamentals.
2. Practice [`02_Files_and_Directories.md`](02_Files_and_Directories.md) and [`03_Viewing_and_Editing.md`](03_Viewing_and_Editing.md).
3. Learn pipelines in [`04_Text_Processing.md`](04_Text_Processing.md).
4. Study monitoring and troubleshooting in [`05_Processes_and_Resources.md`](05_Processes_and_Resources.md).
5. Learn access control in [`06_Users_Groups_and_Permissions.md`](06_Users_Groups_and_Permissions.md).
6. Practice packaging and remote workflows in [`07_Archives_and_Remote_Transfers.md`](07_Archives_and_Remote_Transfers.md).
7. Move to [`08_Networking_and_HTTP.md`](08_Networking_and_HTTP.md) for connectivity and API troubleshooting.
8. Review [`09_System_and_Packages.md`](09_System_and_Packages.md), then study storage/LVM in [`10_Storage_and_LVM.md`](10_Storage_and_LVM.md).

## Safety rules

- Practice destructive commands inside a disposable VM, WSL environment, or lab directory.
- Before `rm`, `mkfs`, `pvcreate`, `mount`, firewall changes, or recursive ownership changes, verify the target and have a rollback/backup plan.
- Do not expose passwords, private keys, API tokens, or cloud credentials in commands or shell history.
- Run `rsync --dry-run` before using `--delete`.
- Scan only systems and networks for which you have authorization.

## Original quick reference

[`Linux_for_DevOps_Day_2_to_Last_Day_Commands.md`](Linux_for_DevOps_Day_2_to_Last_Day_Commands.md) remains available as the compact source checklist.
