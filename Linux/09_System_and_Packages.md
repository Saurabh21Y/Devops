# Linux Notes — System Information and Packages

> Commands for understanding the host, sessions, uptime, shutdown, and installing software.

## 1. `date` — display date and time

```bash
date
date -u
date -I
date -R
date -d 'tomorrow 09:00'
```

`-u` displays UTC, `-I` uses ISO-8601 format, `-R` uses RFC format, and `-d`/`--date` formats a specified date/time. UTC and ISO-8601 timestamps are especially useful for distributed logs and CI/CD systems.

## 2. `uname` — kernel/system information

```bash
uname -a
uname -s
uname -r
uname -m
uname -n
```

`-a` prints all available information; `-s` kernel name, `-r` kernel release, `-m` machine architecture, and `-n` network hostname.

## 3. `uptime` — runtime and load

```bash
uptime
uptime -p
uptime -s
```

It shows how long the system has been running, logged-in users, and load averages. Load average is not a direct CPU percentage; interpret it alongside CPU count and `top`/`vmstat`.

## 4. `shutdown` and `reboot`

```bash
sudo shutdown -h now
sudo shutdown -r +5 'Maintenance reboot in five minutes'
sudo shutdown -c
sudo reboot
```

`shutdown -h` powers off, `-r` reboots, `-c` cancels a scheduled shutdown, and `-k` broadcasts a warning without shutting down. `reboot -f` forces a reboot and should be avoided unless normal shutdown is unavailable.

> Confirm the host, maintenance window, active sessions, and application behavior before shutting down a shared or production machine.

## 5. Package managers

### Debian/Ubuntu: `apt`

```bash
sudo apt update
apt search nginx
apt show nginx
sudo apt install nginx
sudo apt upgrade
sudo apt remove nginx
```

`apt update` refreshes package indexes; it does not upgrade packages. `apt upgrade` installs available upgrades. Use `apt` interactively and `apt-get` in scripts when stable scripting behavior is required.

### Fedora/RHEL family: `dnf`

```bash
sudo dnf check-update
sudo dnf search nginx
sudo dnf info nginx
sudo dnf install nginx
sudo dnf update
sudo dnf remove nginx
```

### Arch: `pacman`

```bash
sudo pacman -Syu
sudo pacman -S nginx
pacman -Ss nginx
pacman -Qi nginx
sudo pacman -R nginx
```

`-S` synchronizes/installs, `-R` removes, `-Syu` synchronizes repositories and upgrades, `-Ss` searches, and `-Qi` queries installed package information.

### Gentoo: Portage

Portage is Gentoo's package-management system. Common workflows use `emerge`, with package selection and compilation controlled by the distribution's configuration.

## Package-management habits

- Identify the distribution before using a package command.
- Refresh package metadata before installing/upgrading.
- Review dependencies and proposed removals.
- Use approved repositories and pin versions when reproducibility matters.
- Apply security updates through a tested maintenance process.
- Record installed versions for incident recovery and build reproducibility.

## Basic host inspection

```bash
uname -a
uptime -p
whoami
id
hostname -I
```
