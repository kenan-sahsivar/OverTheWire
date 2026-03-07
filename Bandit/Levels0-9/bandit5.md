# Bandit Level 4 → 5
## Level Goal
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

## Level 4 → 5 Solution
```bash
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
[REDACTED]
```

### Explanation
Similar to the previous level, we have to first change the directory to `inhere`. The password is in the only file that is human-readable. Human-readable in this context refers to `ASCII text`. Generally whenever you create a file with plain text, it will default to this file type.

Once you're in `inhere`, you may use `ls -a` to check for all the files in the directory, however there are 9, and guessing can take quite some time. Instead, we can use the `file` command to check for the filetype of a file. We can also specify for the shell to check the filetypes of *all* the files in the current directory by using `./*`. The only file that is readable is `-file07`, so we can simply `cat` the file and obtain the password.

**Commands Learned:** `file`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
