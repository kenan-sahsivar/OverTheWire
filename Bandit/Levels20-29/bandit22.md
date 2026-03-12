# Bandit Level 21 → 22
## Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

## Level 21 → 22 Solution
```bash
bandit21@bandit:~$ ls -l /etc/cron.d/
-r--r----- 1 root root  47 Oct 14 09:26 behemoth4_cleanup
-rw-r--r-- 1 root root 123 Oct 14 09:19 clean_tmp
-rw-r--r-- 1 root root 120 Oct 14 09:26 cronjob_bandit22 # <-- bingo
-rw-r--r-- 1 root root 122 Oct 14 09:26 cronjob_bandit23
-rw-r--r-- 1 root root 120 Oct 14 09:26 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8  2024 e2scrub_all
-r--r----- 1 root root  48 Oct 14 09:27 leviathan5_cleanup
-rw------- 1 root root 138 Oct 14 09:28 manpage3_resetpw_job
-rwx------ 1 root root  52 Oct 14 09:29 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
[REDACTED]
```

### Explanation
Once again we're led down a rabbit hole of redirects. To start, `cron` is basically just a task scheduler. What is it scheduling? Well we can check in the directory `etc/cron.d/`. We can see each "cronjob" here, and the most relevant one is `cronjob_bandit22`. To read it we can use `cat`, and upon inspection, the daemon is running the file /usr/bin/cronjob_bandit22.sh and sending any messages right into the universal trash bin (/dev/null). 

Checking what the executable file runs, we see that the file `/tmp/t706...` is constantly being updated. Specifically, the permissions are constantly being updated, and the contents of the file are constantly being overwritten by the password to `bandit22`. As you can guess, if we check the contents of this file, we get the password to `bandit22`. Pretty simple, but expect to see more of `cron`. 

**Commands Learned:** `cron`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
