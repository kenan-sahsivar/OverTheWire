# Bandit Level 25 → 26
## Level Goal
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

## Level 25 → 26 Solution
```bash
bandit25@bandit:~$ ls
bandit26.sshkey
bandit25@bandit:~$ cat bandit26.sshkey
[RSA KEY] # <-- copy this
bandit25@bandit:~$ grep 'bandit26' /etc/passwd
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
bandit25@bandit:~$ exit
```
On your own machine:
```bash
user@machine:~$ echo '[RSA KEY]' > rsakey
user@machine:~$ chmod 400 rsakey
# minimize your screen (a lot)
user@machine:~$ ssh -i rsakey bandit26@bandit.labs.overthewire.org -p 2220
...
--More--(73%) # <-- press v
~
...
:set shell=/bin/bash
:sh
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
[REDACTED]
```


### Explanation
This level starts to get you in the mind of bypassing certain restrictions. The first thing we do is copy the private rsa key in the home directory of `bandit25` to your own machine. Secondly, we need to check what shell `bandit26` is using. To do this, we can check the file `/etc/passwd` which is like a phonebook that stores information about all users, including their home directory and shell. Checking it, we see that instead of the default shell, they're using `/usr/bin/showtext`. This is clearly a script. Upon reading it, there isn't much to see, all it does is display a text file using `more`, and then exits afterward. 

What we need to do is get access to the default shell before the script gets to the `exit 0` line. Since we can't run anything before the script runs, we have to do whatever we need to _while_ the script is running. How? We have to essentially suspend the script on a line, and the one command we can do that with is `more`. 

More on `more` (pun intended), it's used to display the contents of a file in readable portions and without dumping the entire file at once. This means that if the file is too large, more will section the file into multiple parts that you can comfortably read on your screen. If you haven't understood already, while `more` has sectioned a text file into multiple parts, as long as the user doesn't cancel the command or scroll all the way down, `more` stays active. This means that the script won't get to the `exit 0` line as long as we can keep `more` running. Since the text file isn't actually that big, `more` can print it all immediately. However if we reduce our terminal size, `more` won't be able to fit the file, and will then therefore stay running.

Once `more` is essentially paused, what can we do? Well by using the command `v` while in more, it'll open up an editor. The default editor is `vi`, and this is what `more` will be running. If you recall from last level, we can run commands in this editor. The first command we'll run is `:set shell=/bin/bash`, which will change the shell to `bash`, something we can actually use. Once we set up a shell we can use, we can simply run `:sh` to go back to the shell.

Once we have access to the shell, simply check for the password in `/etc/bandit_pass/bandit26`. 

**Commands Learned:** `more`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
