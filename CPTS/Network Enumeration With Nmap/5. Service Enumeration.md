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
