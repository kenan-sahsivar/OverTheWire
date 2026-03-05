# Bandit Level 0 → 1
## Level Goal
The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

## Level 0 → 1 Solution
```bash
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
[REDACTED]
```
### Explanation
Now that we are in, we can start by taking a look in the home directory. The command `ls` (list) is used to list out the contents of the current directory. A directory is a folder that can store either other directories or files inside of it.  Using the command, we can see that there is a file named `readme`.

To read this file, we can use `cat` (concatenate) to write out the contents of the file. Luckily for us, the password is stored in the file, and is written out for us to copy.

**Commands Learned:** `ls` `cat`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
