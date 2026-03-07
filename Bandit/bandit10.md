# Bandit Level 9 → 10
## Level Goal
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

## Level 9 → 10 Solution
```bash
bandit9@bandit:~$ strings data.txt | grep '==='
========== the
========== password
f\Z'========== is
========== [REDACTED]
```

### Explanation
This level we get to use `grep` again. This level really has multiple solutions, so don't be surprised if you used a different method. If you were to `cat data.txt`, you'd see that it's filled with non human-readable text, and we aren't able to take a good look at it. We're told that the password is in one of the few human-readable strings, so you should already think of how we could be finding these strings. That's where the `strings` command comes in. 

When `strings` is used, the output is only the readable strings/lines in that file. This is great, however we're still not able to find our password. It's also been told that the password has a list of '=' characters preceding it. If you recall from bandit7, the `grep` command is used to search within a file. What we're searching for are a list of '=' characters, in this case I used three. To finish it all off, we can use the `|` (pipe) operator to use the stdout of `strings` to the stdin of `grep`. 

**Commands Learned:** `strings`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
