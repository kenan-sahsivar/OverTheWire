# Bandit Level 3 → 4
## Level Goal
The password for the next level is stored in a hidden file in the inhere directory.

## Level 3 → 4 Solution
```bash
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls -a
. .. ...Hiding-From-You
bandit3@bandit:~/inhere$ cat ...Hiding-From-You 
[REDACTED]
```

### Explanation
This time, the password file is not in our current directory. This means that we have to *change* directories. The command for this is `cd`. You may either use a filepath, or if the directory is a child of your current directory, you can simply name it. In this case, we use `cd inhere` to change directories to the `inhere` directory. 

Since the file we're looking for is hidden, a simple `ls` won't work. Hidden files, also sometimes called dotfiles, are files that start with `.` and are hidden by default. To view them, we can use the flag `-a` to view *all* files in the current directory. From here it is straightforward, and you can use `cat` to read the file.

**Commands Learned:** `cd`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
