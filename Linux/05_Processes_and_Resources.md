# Linux Notes — Processes and System Resources

> Commands for inspecting CPU, memory, processes, disk capacity, and long-running jobs.

## 1. Process basics

A **process** is a running instance of a program. Every process has a PID (process ID). PID 1 is the first userspace process and commonly manages services and adopts orphaned processes.

Common states include running, sleeping, stopped, terminated, and zombie.

## 2. `ps` — snapshot of processes

```bash
ps
ps aux
ps -ef
ps -u deploy
ps -p 1234 -f
```

| Option/style | Meaning                                          |
| ------------ | ------------------------------------------------ |
| `-e`         | All processes                                    |
| `-f`         | Full-format listing                              |
| `-u USER`    | Processes for a user                             |
| `-x`         | Include processes without a controlling terminal |
| `a`          | Processes for all users with terminals           |

|

`ps aux` is a common BSD-style view. `ps -ef` is a common full-format view.

## 3. `top` — live process monitor

```bash
top
top -p 1234
top -u deploy
top -d 5 -n 3 -b
```

- `-p PID` watches one process.
- `-u USER` filters by user.
- `-d SEC` sets refresh delay.
- `-n NUM` limits iterations.
- `-b` uses batch output, useful for scripts.

Inside `top`, `P` sorts by CPU, `M` by memory, `k` sends a signal, and `q` exits. Use `k` carefully.

## 4. `kill` — send a signal

```bash
kill -TERM 1234
kill -HUP 1234
kill -KILL 1234
kill -l
```

- `TERM` requests graceful shutdown.
- `HUP` may ask a daemon to reload configuration.
- `KILL`/`-9` stops immediately and cannot be handled by the process.

Prefer `TERM` first, inspect the result, and use `KILL` only when necessary. A signal is not always a literal “kill”; many signals request a state change or reload.

## 5. `fuser` — identify resource users

```bash
fuser -v /var/log/app.log
fuser -m /mnt/data
fuser -u -m /mnt/data
fuser -k -m /mnt/data
```

`fuser` identifies processes using a file, directory, mount, or network namespace. `-k` can terminate those processes, so confirm the target before using it.

## 6. `free` — memory and swap

```bash
free
free -h
free -m
free -t
free -h -s 5
```

`-h` is human-readable, `-m` displays MiB, `-g` GiB, `-t` includes totals, and `-s N` refreshes every N seconds. Pay attention to available memory and swap activity, not only the used column.

## 7. `vmstat` — system activity

```bash
vmstat
vmstat 5 10
vmstat -a
vmstat -s
vmstat -d
```

`vmstat` reports process queues, memory, paging, block I/O, interrupts, and CPU activity. The first output can describe averages since boot; repeated output is better for current trends.

## 8. `df` — filesystem capacity

```bash
df -h
df -hT
df -i
```

- `-h` uses readable units.
- `-T` shows filesystem type.
- `-a` includes pseudo/all filesystems.
- `-i` shows inode usage.
- `-x TYPE` excludes a filesystem type.

A disk can have free bytes but no free inodes, so check both bytes and inodes during “disk full” incidents.

## 9. `du` — directory/file usage

```bash
du -sh /var/log
du -ah /var/log | sort -h | tail
du -h --max-depth=1 /var
```

- `-s` gives a summary.
- `-h` uses readable units.
- `-a` includes files.
- `-c` shows a grand total.
- `--max-depth=N` limits recursion.

`df` answers “how full is the mounted filesystem?”; `du` answers “which files/directories consume space?”

## 10. `nohup` — survive terminal logout

```bash
nohup ./deploy.sh > deploy.log 2>&1 &
```

`nohup` ignores hangup signals. The trailing `&` puts the process in the background, and redirection captures output. For production services, use a service manager such as systemd rather than relying on `nohup`.

## Troubleshooting workflow

```bash
ps aux --sort=-%cpu | head
free -h
df -h
du -h --max-depth=1 /var | sort -h
vmstat 1 5
```

## Quick revision

- `ps` → process snapshot
- `top` → live process/resource view
- `kill` → send process signal
- `fuser` → identify resource users
- `free` → RAM/swap
- `vmstat` → CPU/memory/I/O activity
- `df` → filesystem capacity
- `du` → space used by files/directories
- `nohup` → keep a command running after logout
