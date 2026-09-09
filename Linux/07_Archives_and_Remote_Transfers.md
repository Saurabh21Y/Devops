# Linux Notes — Archives, Compression, and Remote Transfers

> Commands for packaging releases, compressing files, and moving data over SSH.

## 1. `zip` and `unzip`

```bash
zip release.zip app.conf deploy.sh
zip -r release.zip application/
unzip -l release.zip
unzip release.zip -d extracted/
unzip -n release.zip -d safe-extract/
```

`zip -r` includes directories, `-9` requests maximum compression, `-q` is quiet, `-u` updates an archive, and `-e` encrypts a ZIP archive with a password. `unzip -l` lists contents, `-d` selects a destination, `-o` overwrites without prompting, `-n` never overwrites, and `-q` is quiet.

Treat password-protected ZIP files as convenient packaging, not a replacement for a secure secrets-management system.

## 2. `gzip` and `gunzip`

```bash
gzip access.log
gzip -k access.log
gzip -9 large.txt
gunzip access.log.gz
gunzip -c access.log.gz | less
```

`gzip` normally replaces the original with a `.gz` file. `-k` keeps it, `-c` writes compressed/decompressed content to standard output, and `-1` through `-9` choose speed/compression trade-offs. `gunzip -t file.gz` tests integrity without extracting.

Gzip compresses individual streams; it does not package a directory tree by itself. Use `tar` plus gzip for that.

## 3. `tar` — archive trees

```bash
tar -czvf app-2026-09-09.tar.gz application/
tar -tzvf app-2026-09-09.tar.gz
tar -xzvf app-2026-09-09.tar.gz -C /opt/app/
tar -cJf archive.tar.xz application/
```

Classic flags:

| Flag      | Meaning                           |
| --------- | --------------------------------- |
| `-c`      | Create                            |
| `-x`      | Extract                           |
| `-t`      | List contents                     |
| `-v`      | Verbose                           |
| `-f FILE` | Archive filename                  |
| `-z`      | gzip compression                  |
| `-j`      | bzip2 compression                 |
| `-J`      | xz compression                    |
| `-C DIR`  | Change directory before operation |

|

Always inspect an archive with `tar -t` before extracting untrusted content. Use a dedicated destination to avoid overwriting important files.

## 4. `ssh` — secure remote shell

```bash
ssh user@server
ssh -i ~/.ssh/prod.pem -p 2222 user@server
ssh -v user@server
ssh -L 8080:localhost:8080 user@server
ssh -N -L 5432:db.internal:5432 user@bastion
```

- `-i` selects a private key.
- `-p` selects the SSH port.
- `-v` enables connection debugging.
- `-L` forwards a local port through the remote host.
- `-N` does not execute a remote command, useful for tunnels.

Protect private keys with correct permissions, verify host keys, and avoid disabling host-key checking casually.

## 5. `scp` — simple secure copy

```bash
scp -i key.pem build.tar.gz user@server:/tmp/
scp user@server:/var/log/app.log ./
scp -r application/ user@server:/opt/
scp -P 2222 -p config user@server:/etc/app/
```

`-r` copies directories, `-P` sets the SSH port, `-i` selects a key, `-p` preserves times/modes, and `-v` enables debugging. `scp` is convenient for one-off copies; it does not efficiently synchronize repeated transfers.

## 6. `rsync` — efficient synchronization

```bash
rsync -avz application/ user@server:/opt/application/
rsync -av --delete ./dist/ user@server:/var/www/html/
rsync -avP user@server:/var/log/app/ ./logs/
rsync -navi source/ destination/   # dry run
```

- `-a` archive mode (recursive plus metadata)
- `-v` verbose
- `-z` compress during transfer
- `-r` recursive
- `-P` progress and partial files
- `--delete` removes destination files absent from source
- `-n` dry run

Use a dry run before `--delete`. The trailing slash matters: `source/` copies the contents, while `source` can create a directory named `source` at the destination.

## Release packaging workflow

```bash
tar -czf app-release.tar.gz --exclude='*.log' application/
scp app-release.tar.gz deploy@server:/tmp/
ssh deploy@server 'mkdir -p /opt/releases/2026-09-09 && tar -xzf /tmp/app-release.tar.gz -C /opt/releases/2026-09-09'
```

For repeatable production delivery, prefer CI/CD and artifact storage over ad-hoc manual copies.
