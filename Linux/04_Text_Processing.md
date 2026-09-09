# Linux Notes — Text Processing

> Commands for filtering logs, selecting fields, transforming text, comparing files, and building shell pipelines.

## 1. Pipelines and redirection

A pipe sends standard output from one command to the next:

```bash
ps aux | grep nginx
cat access.log | awk '{print $1}' | sort | uniq -c
```

Redirection:

```bash
command > output.txt      # replace file
command >> output.txt     # append
command 2> errors.txt     # stderr only
command > all.txt 2>&1    # stdout and stderr
```

## 2. `grep` — search text

```bash
grep "ERROR" app.log
grep -in "timeout" app.log
grep -r "server_name" /etc/nginx/
```

| Option          | Meaning                         |
| --------------- | ------------------------------- |
| `-i`            | Ignore case                     |
| `-n`            | Show line numbers               |
| `-r` / `-R`     | Search recursively              |
| `-v`            | Show non-matching lines         |
| `-c`            | Count matching lines            |
| `-w`            | Match a whole word              |
| `-E`            | Extended regular expressions    |
| `-A N` / `-B N` | Show lines after/before a match |

|

Examples:

```bash
grep -iE 'error|critical|failed' app.log
grep -RIn --exclude='*.tmp' 'TODO' .
ps aux | grep '[n]ginx'       # avoids matching the grep command itself
```

## 3. `cut` — select columns or character ranges

```bash
cut -d: -f1 /etc/passwd
cut -d, -f1,3 users.csv
cut -c1-20 app.log
```

- `-d` sets the delimiter.
- `-f` selects fields.
- `-c` selects character positions.
- `-b` selects byte positions.
- `--complement` selects everything except the requested fields.

`cut` works best with simple, consistently delimited data.

## 4. `sort` — order lines

```bash
sort names.txt
sort -n response-times.txt
sort -t, -k3,3n users.csv
sort -fu names.txt
```

| Option    | Meaning                                |
| --------- | -------------------------------------- |
| `-r`      | Reverse order                          |
| `-n`      | Numeric order                          |
| `-f`      | Ignore case                            |
| `-u`      | Remove duplicate adjacent sorted lines |
| `-k`      | Sort by a field/key                    |
| `-t CHAR` | Set field separator                    |

|

## 5. `wc` — count lines, words, and bytes

```bash
wc app.log
wc -l app.log
wc -w README.md
wc -c binary-or-text-file
wc -L app.log
```

`-l` counts lines, `-w` words, `-c` bytes, `-m` characters, and `-L` the longest line.

## 6. `awk` — structured text processing

`awk` treats each input line as a record and splits it into fields. `$0` is the full line, `$1`, `$2`, etc. are fields, `NR` is the record number, and `NF` is the number of fields.

```bash
awk '{print $1}' access.log
awk -F: '{print $1, $7}' /etc/passwd
awk '$9 >= 500 {print $0}' access.log
awk 'NR > 1 {total += $3} END {print total}' report.csv
```

Useful patterns:

```bash
awk 'BEGIN {print "START"} /ERROR/ {count++} END {print count}' app.log
awk -F, -v environment=prod '$2 == environment {print $1}' deployments.csv
```

- `-F` sets the field separator.
- `-v name=value` defines a variable.
- `-f file.awk` loads a program from a file.

For complicated parsing, use a script file instead of an unreadable one-liner.

## 7. `sed` — stream editing

```bash
sed 's/old/new/g' config.txt
sed -n '10,20p' app.log
sed '/DEBUG/d' app.log
sed -i.bak 's/localhost/api.internal/g' app.conf
```

Important options:

| Option | Meaning                       |
| ------ | ----------------------------- |
| `-n`   | Suppress automatic printing   |
| `-e`   | Provide an editing expression |
| `-f`   | Read expressions from a file  |
| `-i`   | Edit in place                 |
| `-E`   | Extended regular expressions  |

|

The substitution form is `s/old/new/g`: `s` means substitute and `g` means every occurrence on a line. Prefer `-i.bak` for a backup when modifying important files.

## 8. `diff` — compare files/directories

```bash
diff old.conf new.conf
diff -u old.conf new.conf
diff -y old.conf new.conf
diff -r release-a/ release-b/
```

- `-u` produces the common unified diff format.
- `-y` shows side-by-side output.
- `-q` reports only whether files differ.
- `-r` compares directories recursively.

A zero exit status means no differences; a non-zero status can mean differences, so scripts should handle it intentionally.

## 9. Useful log-analysis pipeline

```bash
# Count HTTP status codes in an access log
awk '{print $9}' access.log | sort | uniq -c | sort -nr

# Show the newest error lines
 grep -i error app.log | tail -n 20

# Print unique failed usernames
awk -F: '/failed/ {print $2}' auth.log | sort -u
```

## Quick revision

- `grep` → find matching lines
- `cut` → select simple fields/characters
- `sort` → order data
- `wc` → count data
- `awk` → field-aware processing and reports
- `sed` → transform/edit streams
- `diff` → compare versions
