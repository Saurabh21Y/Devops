# Linux Notes — Files and Directories

> Commands for moving around the filesystem, creating, copying, renaming, and deleting files/directories.

## 1. Linux paths

Linux uses `/` as the root directory. A path beginning with `/` is **absolute**; a path without it is **relative** to the current directory.

```text
/var/log/app.log   absolute path
./app.log          current directory
../app.log         parent directory
~                  current user's home directory
```

Useful filesystem locations:

| Path       | Purpose                            |
| ---------- | ---------------------------------- |
| `/`        | Filesystem root                    |
| `/home`    | User home directories              |
| `/etc`     | Configuration                      |
| `/var/log` | Logs and variable application data |
| `/tmp`     | Temporary files                    |
| `/opt`     | Optional third-party software      |

## 2. `pwd` — show current directory

`pwd` means **print working directory**.

```bash
pwd
pwd -P       # show the physical path, resolving symbolic links
```

Use it before running a command against a relative path, especially in deployment scripts.

## 3. `ls` — list directory contents

```bash
ls
ls /var/log
ls -lah /var/log
```

Important options:

| Option | Meaning                                      |
| ------ | -------------------------------------------- |
| `-l`   | Long listing: permissions, owner, size, time |
| `-a`   | Include hidden files beginning with `.`      |
| `-h`   | Human-readable sizes with `-l`               |
| `-R`   | Recursively list subdirectories              |
| `-t`   | Sort by modification time                    |
| `-S`   | Sort by size                                 |

|

Common DevOps checks:

```bash
ls -lah                 # inspect everything in the current directory
ls -lt /var/log         # newest files first
ls -ld /etc/nginx       # inspect the directory itself, not its contents
```

## 4. `cd` — change directory

```bash
cd /var/log     # absolute path
cd ..           # one level up
cd ~            # home directory
cd -            # previous directory
cd /            # filesystem root
```

`cd` changes the shell's current location; it does not create a new process.

## 5. `mkdir` — create directories

```bash
mkdir reports
mkdir -p project/logs/archive
mkdir -pv project/{app,logs,tmp}
```

| Option    | Meaning                                                             |
| --------- | ------------------------------------------------------------------- |
| `-p`      | Create missing parent directories and avoid an error if they exist  |
| `-v`      | Print each created directory                                        |
| `-m MODE` | Set permissions during creation, for example `mkdir -m 700 secrets` |

## 6. `touch` — create files or update timestamps

```bash
touch app.log
 touch -d '2026-01-01 10:00' release.txt
 touch -r template.txt copy.txt
```

| Option    | Meaning                           |
| --------- | --------------------------------- |
| `-a`      | Change access time only           |
| `-m`      | Change modification time only     |
| `-c`      | Do not create a missing file      |
| `-d DATE` | Use a specified date/time         |
| `-r FILE` | Copy timestamps from another file |

`touch` does not erase existing content.

## 7. `cp` — copy files and directories

```bash
cp app.conf app.conf.backup
cp -r website/ website-backup/
cp -av source/ /opt/application/
```

| Option      | Meaning                                     |
| ----------- | ------------------------------------------- |
| `-r` / `-R` | Copy directories recursively                |
| `-i`        | Ask before overwriting                      |
| `-u`        | Copy only when the source is newer          |
| `-p`        | Preserve mode, ownership, and timestamps    |
| `-a`        | Archive mode; preserve metadata and recurse |
| `-v`        | Show copied items                           |

For production backups, prefer `cp -a` when preserving metadata matters.

## 8. `mv` — move or rename

```bash
mv old-name.txt new-name.txt
mv build/ /opt/releases/build-2026-01/
mv -i config.new /etc/app/config
```

| Option | Meaning                                 |
| ------ | --------------------------------------- |
| `-i`   | Ask before overwrite                    |
| `-f`   | Force overwrite                         |
| `-n`   | Never overwrite an existing destination |
| `-u`   | Move only when the source is newer      |
| `-v`   | Show moved items                        |

Renaming within one filesystem is usually fast because the directory entry changes rather than file contents being copied.

## 9. `rm` — remove files/directories

```bash
rm temporary.txt
rm -i *.log
rm -r old-release/
```

| Option      | Meaning                         |
| ----------- | ------------------------------- |
| `-r` / `-R` | Remove directories recursively  |
| `-f`        | Force removal without prompts   |
| `-i`        | Ask before each removal         |
| `-I`        | Ask once before a large removal |
| `-v`        | Print removed items             |

> **Warning:** `rm -rf` is immediate and destructive. Verify `pwd`, the target path, and variables before using it in a script. Never test cleanup commands against `/` or an unknown variable.

## 10. `rmdir` — remove empty directories

```bash
rmdir empty-folder
rmdir -p project/logs/archive
```

`rmdir` is safer than `rm -r` because it refuses to remove a non-empty directory.

## 11. `ln` — create links

```bash
ln original.txt hard-link.txt
ln -s /opt/app/current/logs app-logs
```

- A **hard link** points to the same filesystem inode and usually cannot cross filesystems.
- A **symbolic link** stores a path and can point across filesystems, but becomes broken if its target disappears.

Useful options:

| Option | Meaning                         |
| ------ | ------------------------------- |
| `-s`   | Create a symbolic link          |
| `-f`   | Replace an existing destination |
| `-i`   | Ask before replacement          |
| `-v`   | Verbose output                  |

|

Inspect links with `ls -l`.

## 12. `clear`

```bash
clear
```

Clears the visible terminal screen. It does not delete command history or files.

## Practical workflow

```bash
pwd
mkdir -p ~/devops-lab/{input,output,backup}
touch ~/devops-lab/input/app.log
cp -a ~/devops-lab/input/app.log ~/devops-lab/backup/
mv ~/devops-lab/input/app.log ~/devops-lab/output/
ls -lah ~/devops-lab/output
```

## Quick revision

- `pwd` → where am I?
- `ls` → what is here?
- `cd` → move
- `mkdir` / `touch` → create
- `cp` → copy
- `mv` → move/rename
- `rm` / `rmdir` → delete
- `ln` → link
