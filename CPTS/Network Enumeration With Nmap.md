## Introduction to Nmap

**Network mapper.**

- Syntax:

```bash
nmap <scan-types> <options> <target>
```

```bash
nmap --help
```

## Host Discovery

- The most effective host discovery method is ICMP echo requests.
- Store every scan.

### Scan Network Range

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d " " -f5
```

- sn → disable port scan
- oA → store results

### Scan IP List

```bash
sudo nmap -sn -oA tnet -iL hosts.list | grep for | cut -d " " -f6
```

- Performs defined scans.

### Scanning Multiple IPs

```bash
sudo nmap -sn -oA tnet ip1 ip2 ip3 | grep for | cut -d " " -f5
```

or

```bash
10.129.2.18-20
```

- Performs the ping request using ICMP Echo Request against the target.

```bash
sudo nmap ip -sn -oA host -PE --packet-trace
```

- Show all packets sent & received.

```bash
sudo nmap ip -sn -oA host -PE --reason
```

- Display the reason for specific result

```bash
sudo nmap ip -sn -oA host -PE --packet-trace --disable-arp-ping
```

- Disable ARP ping.

**TTL:**

- TTL is a value in an IP packet that decreases by 1 every time it passes through a router.
- TTL helps guess the target OS.
    - 64 → Linux
    - 128 → Windows
    - 255 → Network devices

## Host and Port Scanning

**There are 6 different states for a scanned port:**

- Open
- Closed
- Filtered
- Unfiltered
- Open|Filtered
- Closed|Filtered

### Discovering Open TCP Ports

- Default scan: Top 1000 ports.
- As root: Uses SYN scan (-sS)
- As normal user: Uses TCP Connect scan (-sT)

**Options:**

- -p 22,80
- -p20-100
- -p- (scan all ports)
- -F (Fast scan)
- --top-ports=100 (100 most common TCP ports)

<aside>

RCVD = Packet received
RSA  = RST + ACK flags
S    = SYN

</aside>

### TCP Connect Scan (-sT)

Complete entire TCP three-way handshake. Process:

**Send SYN → Receive SYN-ACK → Send ACK**

If:

- SYN-ACK → Port open
- RST → Port closed

### Filtered Ports

Firewall may be involved.

**Case 1:** Firewall drops packets.

Output: SENT SENT (no reply)

**Case 2:** Firewall rejects packets.

Output: 

ICMP Type 3 Code 3
PORT Unreachable

### Discovering Open UDP Ports

- sU

### Version Detection

- sV

## Saving the Results

**Can save results in 3 different formats.**

**Normal Output**

- oN

**Extension**: .nmap

**Grepable Output**

- oG

**Extension:** .gnmap

**XML**

- oX

**Extension:** .xml

**Save all three formats.**

- oA

**HTML Reports:**

```bash
xsltproc target.xml -o target.html
```

## Service Enumeration

- Service version detection: **-sV**

### **Checking scan progress:**

Full scans can take time. While Nmap is running, press Space.

**Useful options:**

- -stats-every=5s

**Increase verbosity:**

- v

How Nmap detects service versions:

1. Banner Grabbing
2. Signature Matching

After a successful TCP 3-way handshake, many services auto send an identification banner in a packet with PSH (push) flag.

### Manual Banner Grabbing

**Capture traffic with tcpdump:**

```bash
sudo tcpdump -i eth0 host attacker-ip and target-ip
```

**Connect with nc:**

```bash
nc -nv ip port
```

**Captured traffic (tcpdump):**

1. SYN
2. SYN-ACK
3. ACK
4. PSH-ACK (Banner)

## Nmap Scripting Engine

NSE allows to run Lua scripts against target services.

- **Default script:** -sC
- **Run a script category:**

```bash
sudo nmap <target> --script <category>
```

#### Category list:

| Category | Purpose |
| --- | --- |
| **auth** | Finds or tests authentication credentials. |
| **broadcast** | Discovers hosts using broadcast traffic and can automatically add discovered hosts to later scans. |
| **brute** | Performs brute-force login attempts using credentials. |
| **default** | Default scripts executed with `-sC`. |
| **discovery** | Collects information about accessible services. |
| **dos** | Checks for Denial-of-Service (DoS) vulnerabilities. Used less often because it can harm services. |
| **exploit** | Attempts to exploit known vulnerabilities. |
| **external** | Uses external services for additional processing. |
| **fuzzer** | Sends different inputs to discover vulnerabilities or unusual packet handling. Can take a long time. |
| **intrusive** | Runs intrusive scripts that may negatively affect the target. |
| **malware** | Checks whether the target is infected with malware. |
| **safe** | Runs safe, non-intrusive, and non-destructive scripts. |
| **version** | Extends service version detection. |
| **vuln** | Identifies known vulnerabilities. |
- **Run specific scripts:**

```bash
sudo nmap <target> --script <script1>,<script2>,...
```

- **Aggressive scan:** **-A**

Example:

```bash
sudo nmap ip -p 25 --script banner,smtp-commands
```

## Performance

- T (0-5)
- --min-parallelism <number>
- --max-rtt-timeout <time>
- -min-rate <number>
- --max-retries <number>

When Nmap sends a packet, it waits for a response (RTT). By default it's 100 ms.

Example:

```bash
sudo nmap ip/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

### Maximum Retries

Default retry count: 10

### Packet Rate (--min-rate)

Send at least this many packets every second.

### Timing Templates (-T)

- T (0-5)

T3 is normal.

## Firewall and IDS/IPS Evasion

- Firewall
- IDS (Intrusion Detection System)
- IPS (Intrusion Prevention System)

A firewall can handle packets in two ways:

1. Drop packets
2. Reject packets

### ACK Scan (-sA)

Sends only ACK flag, so it's harder to detect.

### Detecting IPS

Use multiple VPS servers with different IPs.

### Decoy Scanning

Hide real IP.

- D RND:5

→ Creates 5 fake IPs.

### Source IP Spoofing (-S)

Sometimes only certain IP ranges are allowed. Manually specify a source IP:

- **S ip**

Combined with:

- **e tun0**

to specify the network interface.

### DNS Proxying

DNS queries usually use UDP 53. Historically, TCP 53 was mainly used for zone transfers or DNS responses larger than 512 bytes.

**Specify your own DNS server:**

```bash
-dns-server <ns1>,<ns2>
```

**Use source port 53:**

```bash
-source-port 53
```

Firewalls often trust traffic coming from DNS port 53.

**Verify with Netcat:**

```bash
ncat -nv --source-port 53 ip port
```
