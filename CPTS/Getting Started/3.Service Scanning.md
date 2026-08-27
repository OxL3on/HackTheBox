**Nmap**

<aside>

nmap --script -sC

</aside>

Attacking Network Service : FTP , SSH , SMB

**SMB (Server Message Block)**

- Allows sharing folders and making them accessible remotely.
- Tool for enumerating and interacting with SMB shares:
    - `smbclient`

Examples:

```
smbclient -N -L \\\\ip
```

Connect to guest user.

```
smbclient -U bob \\\\ip\\users
```

Use `get` to download files.

**SNMP**

Provides information and statistics about a router or device, helping us gain access.

Tools:

- snmpwalk
- onesixtyone
