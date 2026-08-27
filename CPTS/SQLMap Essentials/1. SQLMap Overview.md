## **SQLMap Overview**

```bash
python sqlmap.py -u 'http://inlanefreight.htb/page.php?id=5'
```

**Supported SQL Injection Types : `sqlmap -hh`**

The technique characters `BEUSTQ` refers to the following:

- `B`: Boolean-based blind
- `E`: Error-based
- `U`: Union query-based
- `S`: Stacked queries
- `T`: Time-based blind
- `Q`: Inline queries

UNION query-based is the fastest SQLi type.
