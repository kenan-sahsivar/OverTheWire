# Bandit Level 2 → 3
## Level Goal
The password for the next level is stored in a file called --spaces in this filename-- located in the home directory

## Level 2 → 3 Solution
```bash
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat "spaces in this filename"
[REDACTED]
```
**ALTERNATIVELY**
```bash
bandit2@bandit:~$ cat spaces\ in\ this\ filename
[REDACTED]
```

### Explanation
This one is tricker, since now there are spaces in the filename. If we were to use `cat spaces in this filename`, the shell would interpret it as us trying to cat 4 seperate files. To avoid this issue, we can simply put the filename in quotations. 

Alternatively, you may also use backslashes `\` to "escape" the spaces. Simply use `\` before any spaces to indicate that the space is part of the file name and not an indication to cat seperate files. 

**Commands Learned:** 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
