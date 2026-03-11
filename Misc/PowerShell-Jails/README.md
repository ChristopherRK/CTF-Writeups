# PS-Jail Writeup
## Vulnerability: Space Filter Bypass
Used ${IFS} to bypass space restrictions:
```powershell
{cat${IFS}/flag.txt}
```
