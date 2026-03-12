# Bandit Level 22 → 23
## Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Level 22 → 23 Solution
```bash
bandit22@bandit:~$ ls -l /etc/cron.d/
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$: echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$: cat /tmp/8ca319486bfbbc3663ea0fbe81326349
[REDACTED]
```

### Explanation
Now we're really starting to get into scripting. The start of the solution is the same as the last, we search the `cron.d` directory for active scheduled commands. Since the last one was `cronjob_bandit22`, the one for this level is most likely `cronjob_bandit23`. When we read the file, it's essentially the same thing as the last level, however now the executable file is called `cronjob_bandit23.sh`. When we read the script, we see something interesting. Since the next level requires us to start scripting, I suppose I should explain the fundamentals. 

When starting a script, it's good practice to start with a shebang line. A shebang? A shebang is just these symbols: `#!`. Scripts _have_ to (there are exceptions but you should use it), start with a `#!` because that indicates that it's a script. The following `/bin/bash` is an indicator of which shell we're using. I like to use `zsh`, but the default is `bash`. Some scripts might work on both, however all shells have their little differences, so this is why we use this line, to indicate I guess which "language" you're coding in. It wouldn't make sense to run a javascript file in a python environment.

The next line is `myname=$(whoami)`. This line is declaring a variable called `myname`, and setting the value of it to the stdout of the command `whoami`. As you can guess, the next line creates a variable called `mytarget`, and assigns it the value of the output of a couple commands piped to each other. Let's examine it closely. 

The first section `echo I am user bandit23`, is pretty easy to understand. We're taking "I am user bandit23" and feeding it into the stdin of the next command through a pipe. The next command is one we haven't seen before. In short, `md5sum` creates a hash of a file. If you don't know, a hash in a nutshell is like an encrypted name assigned to a file through an algorithm. It's not impossible for two files to have the same name assigned, however it's extremely rare for it to happen on it's own, and is generally not a problem. 

In this case, we're creating a hash of `'I am user bandit23'`. Once we have the hash, it'll look something like this `randomcharacters -`. What the 3rd and final command does, is that it takes this hash, and removes the ending. If we were to use the file as an argument, the output would look something like `randomcharacters pathtofile`. Since we're feeding it through `stdin`, it doesn't have a file to reference, so it uses `-` to indicate `stdin`. 

To remove this dash, the script uses the command `cut`. Before I explain how it works, a _delimiter_ can be anything from a character, or a string, anything that is used to signify beginning and ends of a data stream. For example in a filepath, `/` would be the delimiter. The command `cut` will take apart sections from it's input argument, which can come from a pipe, or a file. You can choose which section to display by using the `-f` option, followed by whatever section you want. If you don't specify a delimiter, nothing will be cut. If you do, each line is cut seperately. It will cut around whatever field/section is specified. By specifying the delimiter as a space, the script cuts the line into two sections: the hash, and the reference. Since only the hash is wanted, the `field` used is `1`.

Copy this command yourself, and replace $myname with bandit23, as that would be the value of the variable. The hash will come out to be a long list of characters. The script creates a file by the name of this hash in the `/tmp` folder. Check there, and the password is yours to view.

**Commands Learned:** `md5sum` `cut`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
