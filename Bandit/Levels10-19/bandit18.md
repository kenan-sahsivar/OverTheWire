# Bandit Level 17 → 18
## Level Goal
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

## Level 17 → 18 Solution
```bash
bandit17@bandit:~$ diff  passwords.old passwords.new
< pGozC8kOHLkBMOaL0ICPvLV1IjQ5F1VA
---
> [REDACTED]
```

### Explanation
We're taking a quick break out of the whole localhost thing. This level we're doing a simple check to find the line that is different between the two text files.
The command we can use for this is `diff`, which will check for the differences between the lines in a file. The only line that is different, is the password. Just so you know, the line that begins with `<` indicates the first text file (passwords.old in our case), and the line that begins with `>` is the second text file. If we were to instead use `diff passwords.new passwords.old`, the password that we would be looking for would be followed by a `<` since the newer passwords are in the first text file.

**Commands Learned:** `diff`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
