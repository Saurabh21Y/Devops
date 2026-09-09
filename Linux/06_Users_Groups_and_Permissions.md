# Linux Notes — Users, Groups, and Permissions

> Commands for identity, account management, groups, ownership, and access control.

## 1. Identity concepts

Linux permissions are evaluated for the file **owner**, the file's **group**, and **others**. A user can belong to multiple groups.

```text
r = 4 (read)
w = 2 (write)
x = 1 (execute)
```

For directories, read lists entries, write creates/removes entries, and execute allows entering/traversing the directory.

## 2. `whoami`, `id`, and `who`

```bash
whoami
id
id deploy
id -u deploy
id -g deploy
id -Gn deploy
who
who -H
```

- `whoami` shows the effective username.
- `id` shows UID, primary GID, and supplementary groups.
- `who` shows currently logged-in sessions.

`id -u`, `-g`, `-G`, and `-n` select user ID, primary group ID, all group IDs, and names.

## 3. `which` — locate an executable

```bash
which python3
which -a python
```

It searches the directories in `PATH`. Use `command -v tool` in portable shell scripts because aliases/functions can affect `which` behavior.

## 4. `sudo` — run with controlled privilege

```bash
sudo systemctl restart nginx
sudo -u deploy id
sudo -l
sudo -i
sudo -s
sudo -k
```

- `-u USER` runs as another user.
- `-l` lists allowed commands.
- `-i` opens a login shell as the target user.
- `-s` opens a shell while preserving more of the current environment.
- `-k` invalidates cached credentials.

Use the smallest privilege needed. Avoid running an entire application as root.

## 5. `useradd` — create a user

```bash
sudo useradd -m -s /bin/bash deploy
sudo useradd -m -d /srv/app -G docker,developers appuser
sudo useradd -u 1050 -m worker
```

| Option      | Meaning                   |
| ----------- | ------------------------- |
| `-m`        | Create the home directory |
| `-d DIR`    | Set home directory        |
| `-s SHELL`  | Set login shell           |
| `-G GROUPS` | Add supplementary groups  |
| `-u UID`    | Set UID                   |

|

A new account may need a password, SSH key, expiration policy, and appropriate group membership before use.

## 6. `passwd` — manage passwords

```bash
sudo passwd deploy
sudo passwd -l deploy
sudo passwd -u deploy
sudo passwd -e deploy
sudo passwd -S deploy
```

`-l` locks, `-u` unlocks, `-d` removes a password, `-e` forces expiration, and `-S` shows password status. Removing a password is not the same as disabling every possible login method.

## 7. `su` — switch user

```bash
su - deploy
su -c 'id' deploy
su -s /bin/bash deploy
```

`-` or `-l` creates a login shell, `-c` runs one command, and `-s` chooses a shell. `sudo -u` is often easier to audit than sharing a root password.

## 8. `userdel` — remove a user

```bash
sudo userdel deploy
sudo userdel -r deploy
```

`-r` removes the home directory and mail spool. Before deletion, check running processes, owned files outside the home directory, scheduled jobs, and service dependencies.

## 9. Group commands

```bash
sudo groupadd developers
sudo groupadd -r appservice
sudo gpasswd -a deploy developers
sudo gpasswd -d deploy developers
sudo groupdel old-team
```

`groupadd -g GID` chooses a group ID and `-r` creates a system group. `gpasswd -a USER GROUP` adds membership and `-d` removes it. A user may need to log in again before a new group membership appears in the session.

`groupdel GROUP` deletes a group. Confirm that it is not the primary group of an existing user and that no service or shared directory still depends on it.

## 10. `chmod` — change mode bits

```bash
chmod 640 app.conf
chmod 750 deploy.sh
chmod u+x deploy.sh
chmod -R g+rX shared/
```

Numeric permissions are calculated as owner/group/others:

| Number | Permission |
| -----: | ---------- |
|      7 | `rwx`      |
|      6 | `rw-`      |
|      5 | `r-x`      |
|      4 | `r--`      |
|      0 | `---`      |

`chmod 640 app.conf` means owner read/write, group read, others no access. `-R` is recursive; use it cautiously because files and directories often need different execute behavior.

## 11. `chown` — change owner/group

```bash
sudo chown deploy app.log
sudo chown deploy:developers shared/
sudo chown -R deploy:developers /srv/app
```

`-R` recurses, `-v` is verbose, `-c` reports changes, and `--reference=FILE` copies ownership from another file. Verify the path before recursive ownership changes.

## 12. `chgrp` — change group only

```bash
sudo chgrp developers shared/
sudo chgrp -R developers /srv/app
```

It has options similar to `chown`, including `-R`, `-v`, `-c`, `-f`, and `--reference`.

## 13. `umask` — default creation permissions

```bash
umask
umask -S
umask 027
```

`umask` removes permissions from newly created files/directories. It does not directly change existing files. A common restrictive value such as `027` prevents “others” access by default; exact results depend on the program and base permissions.

## 14. `history` — shell command history

```bash
history
history | tail
history -c
history -d 120
history -a
history -w
```

`-c` clears, `-d N` deletes an entry, `-a` appends the current session, `-w` writes history, and `-r` reads it. Do not place secrets or tokens in command arguments; history may retain them.

## Permission troubleshooting

```bash
id appuser
ls -ld /srv/app
ls -l /srv/app/config.yaml
namei -l /srv/app/config.yaml
```

Check every parent directory's execute permission, ownership, group membership, and whether a service is running under the user you expect.
