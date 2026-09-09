# Linux Notes — Viewing and Editing Files

> Commands for reading logs/configuration, inspecting output, and editing text from a terminal.

## 1. `cat` — print or combine files

```bash
cat app.conf
cat part-01 part-02 > complete.txt
cat -n script.sh
```

| Option | Meaning                                                    |
| ------ | ---------------------------------------------------------- |
| `-n`   | Number every line                                          |
| `-b`   | Number only non-empty lines                                |
| `-A`   | Show tabs, line endings, and other non-printing characters |
| `-s`   | Collapse repeated blank lines                              |
| `-E`   | Mark line endings                                          |

For large files, use `less` instead of `cat` so the terminal is not flooded.

## 2. `head` — view the beginning

```bash
head app.log
head -n 20 app.log
head -c 100 app.log
```

`-n` selects lines and `-c` selects bytes. This is useful for checking headers, CSV column names, or the first lines of a deployment log.

## 3. `tail` — view the end or follow logs

```bash
tail -n 50 app.log
tail -f /var/log/nginx/access.log
tail -F /var/log/app/app.log
```

- `-n N` shows the last N lines.
- `-f` follows a file as it grows.
- `-F` follows by filename and handles log rotation/replacement better than `-f`.
- Press `Ctrl+C` to stop following.

Very common troubleshooting pattern:

```bash
tail -f /var/log/myapp/app.log | grep -i error
```

## 4. `less` — interactive file viewer

```bash
less -N /var/log/syslog
```

Useful options:

| Option | Meaning                            |
| ------ | ---------------------------------- |
| `-N`   | Show line numbers                  |
| `-S`   | Do not wrap long lines             |
| `-i`   | Case-insensitive searching         |
| `-X`   | Keep screen content after exit     |
| `-F`   | Exit automatically if content fits |

|

Inside `less`: `/error` searches, `n` goes to the next match, `g` goes to the beginning, `G` goes to the end, and `q` exits.

## 5. `more` — simple pager

```bash
more large-output.txt
```

`more` displays one page at a time. `less` is generally preferred because it supports backward navigation and better searching.

Common options include `-d` for helpful prompts, `-f` for logical line counting, `-s` to squeeze blank lines, and `-c` to repaint instead of scrolling.

## 6. `echo` — print text and variables

```bash
echo "Deployment started"
echo "$HOME"
echo "enabled=true" > app.env
echo "next=value" >> app.env
```

| Option | Meaning                                   |
| ------ | ----------------------------------------- |
| `-n`   | Do not add a trailing newline             |
| `-e`   | Interpret escapes such as `\\n` and `\\t` |
| `-E`   | Disable escape interpretation             |

`>` replaces a file, while `>>` appends. Be careful: `echo value > config` can overwrite configuration.

## 7. `tee` — display and save pipeline output

```bash
echo "health check passed" | tee health.log
make deploy 2>&1 | tee deploy.log
printf '%s\\n' "new line" | tee -a notes.txt
```

- `-a` appends instead of overwriting.
- `-i` ignores interrupts.

`tee` is useful when you want to see command output and retain it for later troubleshooting.

## 8. `vi` / `vim` — terminal editor

```bash
vim app.conf
```

Essential Vim workflow:

1. Press `i` to enter insert mode.
2. Type or edit text.
3. Press `Esc` to return to normal mode.
4. Type `:w` and Enter to save.
5. Type `:q` and Enter to quit.
6. Type `:wq` to save and quit.
7. Type `:q!` to quit without saving.

> Always make a backup before editing a production configuration, and validate the configuration before restarting a service.

## Useful combinations

```bash
# Inspect a configuration with line numbers
cat -n /etc/nginx/nginx.conf | less

# Watch only errors in a live log
tail -F /var/log/app.log | grep -i --line-buffered error

# Save command output while still seeing it
systemctl status nginx 2>&1 | tee nginx-status.txt
```

## Quick revision

- `cat` → print/concatenate small files
- `head` → beginning
- `tail` → end/live logs
- `less` → interactive large-file viewer
- `more` → basic pager
- `echo` → print/write simple values
- `tee` → screen plus file
- `vim` → terminal editing
