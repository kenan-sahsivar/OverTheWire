# Bandit Level 26 → 27
## Level Goal
Good job getting a shell! Now hurry and grab the password for bandit27!

## Level 26 → 27 Solution
```bash
bandit26@bandit:~$ ls
bandit27-do  text.txt
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
[REDACTED]
```

### Explanation
While you're still logged into `bandit26`, simply use the setuid that allows you to run commands as `bandit27` to read the password for the next level.

**Commands Learned:** 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
