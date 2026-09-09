# Linux Notes — Networking and HTTP

> Commands for checking connectivity, interfaces, routes, DNS, ports, HTTP APIs, and firewall rules.

## 1. `hostname`

```bash
hostname
hostname -s
hostname -f
hostname -I
```

`-s` shows the short name, `-f` the fully qualified name, `-i` a resolved address, and `-I` all configured addresses. Changing a hostname usually requires system configuration and may affect certificates or monitoring.

## 2. `ip` — modern network administration

```bash
ip addr
ip link
ip route
ip neigh
ip -s link
```

- `ip addr` shows addresses.
- `ip link` shows interfaces and link state.
- `ip route` shows routes.
- `ip neigh` shows neighbor/ARP information.
- `ip -s link` shows interface statistics.

`ip` replaces many older `ifconfig`, `route`, and `arp` commands.

## 3. `route` — legacy routing table command

```bash
route -n
route -e
```

`-n` avoids DNS lookups and `-e` shows extended information. Prefer `ip route` for current systems. Adding or deleting routes changes traffic flow and should be done only with a clear rollback plan.

## 4. `ping` — reachability and latency

```bash
ping -c 4 example.com
ping -4 -W 2 -c 3 10.0.0.1
```

`-c` limits packets, `-i` sets interval, `-W` sets timeout, `-s` sets packet size, and `-4`/`-6` chooses IP version. A failed ping does not always prove a service is down because firewalls may block ICMP.

## 5. `ss` and `netstat` — sockets and listening ports

```bash
ss -tulnp
ss -tan
ss -ltn 'sport = :443'
netstat -tulnp
```

`-t` TCP, `-u` UDP, `-l` listening, `-n` numeric, and `-p` process information. `ss` is the modern replacement; `netstat` may require the legacy `net-tools` package.

## 6. `traceroute`, `tracepath`, and `mtr`

```bash
traceroute -n example.com
tracepath example.com
mtr -rwzc 10 example.com
```

These reveal network hops and path quality. `-n` avoids DNS lookups; `mtr -r` creates a report, `-c` sets cycles, `-w` uses wide output, and `-i` sets interval. Intermediate hops may not answer probes even when the final service works.

## 7. DNS: `nslookup` and `dig`

```bash
nslookup example.com
nslookup -type=MX example.com
dig example.com A +short
dig @1.1.1.1 example.com
dig -x 192.0.2.10
dig +trace example.com
```

Use `-type`/`-query` for record types such as A, AAAA, MX, NS, or TXT. `dig` is preferred for detailed, scriptable DNS troubleshooting; `+short` keeps output concise.

## 8. `telnet` and `nc` — test TCP/UDP connectivity

```bash
telnet example.com 443
nc -vz -w 3 example.com 443
nc -l 8080
```

`nc` options include `-l` listen, `-v` verbose, `-z` scan without sending data, `-u` UDP, and `-w N` timeout. These tools test port connectivity; a successful TCP connection does not prove the application protocol is healthy.

## 9. `curl` — HTTP/API requests

```bash
curl -I https://example.com
curl -sS https://api.example.com/health
curl -X POST -H 'Content-Type: application/json' -d '{"enabled":true}' https://api.example.com/config
curl -o response.json https://api.example.com/data
```

- `-X` sets the method.
- `-H` adds a header.
- `-d` sends request data.
- `-I` fetches headers only.
- `-o` writes to a file.
- `-sS` is quiet except for errors.
- `-f` fails on HTTP errors.
- `-L` follows redirects.

Never place long-lived secrets directly in shell history. Use environment variables or a secret manager.

## 10. `jq` — process JSON

```bash
curl -sS https://api.example.com/status | jq
curl -sS https://api.example.com/status | jq -r '.status'
curl -sS https://api.example.com/users | jq '.users[] | {id, name}'
```

`-r` prints raw strings, `-c` compact JSON, `-M` disables color, `-S` sorts keys, and `-e` uses expression success in the exit status.

## 11. `wget` — download resources

```bash
wget https://example.com/app.tar.gz
wget -O app.tar.gz https://example.com/download
wget -c URL
wget -P /tmp/downloads URL
```

`-O` chooses output filename, `-c` continues a partial download, `-q` is quiet, `-P` sets a directory prefix, and `-r` recursively downloads. Validate checksums/signatures for release artifacts.

## 12. `arp` and `ifconfig` (legacy tools)

```bash
arp -a
ifconfig -a
```

`arp` inspects legacy ARP cache entries and `ifconfig` displays/configures interfaces. Prefer `ip neigh`, `ip addr`, and `ip link` on modern Linux systems.

## 13. `iwconfig` and `ifplugstatus` (legacy/link tools)

```bash
iwconfig
iwconfig wlan0
ifplugstatus -a
ifplugstatus -i eth0
```

`iwconfig` displays/configures older wireless interfaces (`essid`, `mode`, `channel`, `txpower`, and `rate`). `ifplugstatus` checks whether a physical link is detected; `-a` checks all interfaces, `-i` selects one, `-q` is quiet, and `-v` is verbose. Modern Wi-Fi systems commonly use `iw` and NetworkManager tools instead.

## 14. `whois`

```bash
whois example.com
whois -H example.com
whois -I 192.0.2.10
```

It queries registration information. Results depend on the registry and may be privacy-redacted. `-H` hides legal disclaimers and `-I` performs an IP lookup where supported.

## 15. `iptables` — firewall rules

```bash
sudo iptables -L -n -v
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
```

`-L` lists, `-A` appends, `-I` inserts, `-D` deletes, and `-F` flushes rules.

> **Warning:** Firewall changes can disconnect your SSH session. Understand the default policy, ordering, persistence, and cloud security-group rules before changing production systems.

## 16. `nmap` — authorized port/service discovery

```bash
nmap -p 22,80,443 server.example.com
nmap -sV -p 1-1000 192.0.2.10
nmap -Pn host.example.com
```

`-sV` detects service versions, `-p` chooses ports, `-Pn` skips host discovery, `-O` attempts OS detection, and `-v` increases detail. Scan only systems and networks you own or are explicitly authorized to test.

## 17. `watch` — repeat a command

```bash
watch -n 5 'ss -tulnp'
watch -d df -h
watch -g command
```

`-n` sets the interval, `-d` highlights changes, `-t` hides the header, `-g` exits when output changes, and `-e` exits on command error.

## Connectivity troubleshooting order

```bash
ip addr
ip route
ping -c 3 gateway-ip
ping -c 3 host
ss -tulnp
nc -vz host port
dig host
curl -v --max-time 10 https://host/health
```
