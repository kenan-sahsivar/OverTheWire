# Bandit Level 1 → 2
## Level Goal
The password for the next level is stored in a file called - located in the home directory

## Level 1 → 2 Solution
```bash
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
[REDACTED]
```
### Explanation
The password is located inside of the file `-`. The solution should be pretty simple, to just echo out the contents of `-`, however cat is used for more than just reading out files. This is why when we use `cat -`, it interprets it as us trying to use a flag. To avoid this issue, we can use `./-`. Essentially what this does is that it provides a file path, instead of just the file name. The `./` prefix says to look inside the current directory, while using something like `../` would look in the parent directory (one level up). 

**Commands Learned:** 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
