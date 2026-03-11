# Bandit Level 18 → 19
## Level Goal
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

## Level 18 → 19 Solution
```bash
bandit18@bandit:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
[REDACTED]
```

### Explanation
Right back to the caveats of ssh. In this level, we have the password for the user, and the next password is in a human readable file in the homedirectory of the next user. The twist is that we quite literally can't access it. Since we are logged out immediately after logging in, we aren't able to use any commands. 

I'll explain how to bypass this, but before I do that I should explain what exactly `.bashrc` is. `.bashrc` is a hidden file in the user's home directory, that essentially explains how to load the bash shell, and any configurations made to it. If you use `bash`, then you most likely have this file. I use the `zsh` shell, so instead I have a file called `.zshrc`. 

Now back to the level, if you take a look into the `.bashrc` file, you'll see the following at the end.
```bash
echo 'Byebye !'
exit 0
```
What this does is that it displays 'Byebye !', and then promptly exits the user out of the shell. To bypass this, we have to get a command to run before the shell can fully configure and run these lines. To do that we can edit the `ssh` command, by adding whatever command we'd like to run on startup at the end. Of course you'll still be logged out, however you can run any command you want as the user before it happens. You can see where I'm going with this. If you simply run `cat readme` at the end of your `ssh` command, then the password to the next level will be wrote out in the shell.

**Commands Learned:** 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
