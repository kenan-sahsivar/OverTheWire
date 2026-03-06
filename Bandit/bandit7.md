# Bandit Level 6 → 7
## Level Goal
The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size


## Level 6 → 7 Solution
```bash
bandit6@bandit:~$ find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
[REDACTED]
```

### Explanation
Similar to the previous level, we have to find a file using `file`. However the difference is that now since the search scope has increased to the entire server, we use `/` to search in the root directory. 

As you may have guessed, we use the flags `-user` to check for ownership by a user, and `-group` to check if it belongs to a group. 

Since we're searching through the root directory, there are quite a lot of directories we are not allowed to look in, so trying to use `find` on them will return many error messages that can fill up our terminal. To keep our terminal clean, we can include the `2>/dev/null` command that will redirect these errors. This command is not necessary for the `find` command to work, so it is not required for you to know how it works, however I will explain below.

The `2` in `2>/dev/null` is essentially a file descriptor for error messages. This command will basically only apply to error messages. The `>` symbol is a redirect operation, telling the shell to redirect the error messages. `/dev/null` is the location we're redirecting these messages. Don't worry, it doesn't take up space. This location is essentially a blackhole, and anything in it is immediately discarded/deleted.

**Commands Learned:** `2>/dev/null` *(optional)*

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
