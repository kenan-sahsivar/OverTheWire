# Bandit Level 23 → 24
## Level Goal
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Level 23 → 24 Solution
```bash
 bandit23@bandit:~$ ls /etc/cron.d 
behemoth4_cleanup  cronjob_bandit23  leviathan5_cleanup    sysstat
clean_tmp          cronjob_bandit24  manpage3_resetpw_job
cronjob_bandit22   e2scrub_all       otw-tmp-dir
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash
shopt -s nullglob
myname=$(whoami)

cd /var/spool/"$myname"/foo || exit 
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" "./$i")"
        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi
        rm -rf "./$i"
    fi
done
bandit23@bandit:~$ mktemp -d
/tmp/tempdir
bandit23@bandit:~$ cd /tmp/tempdir
bandit23@bandit:/tmp/tempdir$ touch password.txt script.sh
bandit23@bandit:/tmp/tempdir$ chmod 777 password.txt script.sh ./
bandit23@bandit:/tmp/tempdir$ vi password.txt
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tempdir/password.txt
~
~
bandit23@bandit:/tmp/tempdir$ cp script.sh /var/spool/bandit24/foo/script.sh
# may need to wait a minute
bandit23@bandit:/tmp/tempdir$ cat password.txt
[REDACTED] 
```

### Explanation
Once again, since we're checking for `cron` scripts, we'll be doing the same steps to find the file. First check the directory for all the scheduled tasks, and then check the schedule for `bandit24`. Inside just like always you'll find the script it's referencing, and we can go ahead and read it. Th is level the script is a lot harder to understand, but you should still be able to get a basic understanding of what it's doing assuming you've gotten to this point by yourself. Once again, it's fine if you do not understand, especially if you are relatively new to bash. Pay close attention.

The script starts off with the classic `#!/bin/bash`, indicating that the file is a script. Right afterward, you'll see a command which is interesting, and isn't necessarily important for us, but is a good safety fallback. We'll get back to it later. Right afterward, a variable called `myname` is initialized, with the value of `whoami`, which in this case is `bandit24`. The next line changes directories to `/var/spool/bandit24/foo/`, but if you'll notice, it's followed by `||`, and an `exit` command. Once again, this is a fail safe. 

Even in controlled environments, fail safes are created to prevent disastrous consequences. If you skim through the script, you'll see `rm -rf` being run, which can be dangerous if run in the wrong place. This is because it's used to remove files. This is why the fail safe exists. The `||` is an OR operator, which follows the logic that if the first option fails, it'll try the second one. If you have programming experience, you might remember it as executing code if either conditions are met, but in bash, it's not exactly the same. The command will first try to change directories, however if the command fails, then the other command will run, which is `exit`. 

We can skip the `echo` line, and the next relevant line is the start of the `for` loop. Although I'd love to explain how for loops work and all the details of it, this isn't really the place for it. I've linked a couple articles regarding theory for those of you that are complete beginners. In short, the for loop goes through every file in the `foo` directory. Note that it uses a _wildcard_ to iterate through any files. The problem is that if there are no files in the directory, the script will take `* .` as an actual file name, and once it tries to run a command with this as a file name, the script will promptly break. To avoid this the script uses a shell option (`shopt -s nullglob`) which just returns null instead of `* .` as the file name.

Going back to the for loop, the loop takes into account every file in the directory. _For_ every file, it first checks that it's not doing anything to the current directory, and then that it's not affecting the parent directory. Once it checks that, it creates a variable using a fancy way to check who the owner of the file is. No need to deep dive into that. Once its something that's in the current directory, it checks if `bandit23` owns it, and if it's a regular file (e.c. not a directory). Finally it runs the file/script with the failsafe that if it takes more than 60 seconds to do whatever the script needs to do, it instantly gets terminated.

Anyway that was a really dumbed down explanation of how the script works. If you're really interested, you're welcome to get a step by step explanation from an llm, however this is as best as I can do for the intents of this documentation. Now that we know that this script can run any scripts that we place in the directory, it exposes another privilege escalation vulnerability. If we can get `bandit24` to run any script we want, we can effectively run any command as the user. 

Using this knowledge, I have created a simple script that will move the contents of `/etc/bandit_pass/bandit24` to a file of our choice. I've placed both this password file and the script in a directory using `mktemp -d`. Don't overthink scripts, it's essentially a list of commands that will run in order, until you get to things like `if` statements and `for` loops. I've forgot to mention, but in order to create the script, you'll have to use a text editor like `vi`. Once we make the script, we can simply copy it to a file in the `foo` directory, which will be run by the `cron` task. After waiting a couple of seconds, the long awaited password for the next level should be in the password file that you have made.

_Note: sorry for the rush in this level, it's 3am as I'm typing this, and I don't feel like going into depth. I may go back to this level and expand on the explanation if needed._

**Commands Learned:** `vi` `||` `if` `for` `rm`

**See:** [vi explanation](https://www.cs.colostate.edu/helpdocs/vi.html)

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
