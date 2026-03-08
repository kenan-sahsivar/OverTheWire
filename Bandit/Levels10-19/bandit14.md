# Bandit Level 13 → 14
## Level Goal
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.

## Level 13 → 14 Solution
```bash
bandit13@bandit:~$ cat sshkey.private
[RSA PRIVATE KEY] #copy this
bandit13@bandit:~$ exit
```
_Then on your system_
```bash
me@mylaptop:~$ echo [RSA PRIVATE KEY] > sshkey.private
[RSA PRIVATE KEY]
me@mylaptop:~$ chmod 400 sshkey.private
me@mylaptop:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
_Now on bandit14_
```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
[REDACTED]
```

### Explanation
From now on, the next couple of levels will have to do with remote login and different ways to do so. Up until this level, we've been using `ssh` to login via the user's password, however that's not the only way to do so. Private keys can also be used to for authentication instead of regular passwords.

This level you'll need to work a little bit more on your own system, since OverTheWire doesn't allow localhost connections, like connecting directly from bandit13 to bandit14. First we'll have to copy the `sshkey.private` file, then make a file on our system for it. We can use `echo "text" > file.name`. Using `echo`, we can "echo" the text to the stdout. In this case, the text is the private key. You probably recognize the `>` operator from the previous levels, but if you don't remember, it's used to redirect the stdout into a file. If the file doesn't exist, it is created.

Once our file is created, before we do anything, we need to secure it. Right now anyone can open our file, and for a private key, that's a major concern. To fix it's permissions, we use the `chmod` command to modify the permissions of the file, and `400` to default it to so that only the owner (or root) can open the file.

Now that our file is secure, we can use `ssh` like how we would normally, however we use the `-i` flag, then specify the private key that will be read. Once we're in, simply `cat` the file containing the password.

**Commands Learned:** `echo` `chmod`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
