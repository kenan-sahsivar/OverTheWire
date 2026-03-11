# Bandit Level 19 → 20
## Level Goal
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

## Level 19 → 20 Solution
```bash
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
[REDACTED]
```

### Explanation
Now we're finally using setuid's. As basic as it gets, a setuid binary is an executable file that you can run which will temporarily grant you elevated privileges of the user that owns the setuid binary. For example, if the setuid binary is owned by root, now there's a vulnerability. In our case, `bandit20-do` is owned by bandit20, however we still have permissions to run it since we are in the group. Using it, we can check the password directory (`/etc/bandit_pass/`) for the password of bandit20. To use it, just add the command you want to run with bandit20's privileges, after the executable file.

**Commands Learned:** 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
