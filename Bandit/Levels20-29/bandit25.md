# Bandit Level 24 → 25
## Level Goal
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

## Level 24 → 25 Solution
```bash
bandit24@bandit:~$ mktemp -d
/tmp/tempdir
bandit24@bandit:~$ cd /tmp/tempdir
bandit24@bandit:/tmp/tempdir$ touch log.txt script.sh
bandit24@bandit:/tmp/tempdir$ vi script.sh
#!/bin/bash
for i in {0000..9999}
do
        echo <bandit24 password> $i
done | nc localhost 30002 >> log.txt
~
~
bandit24@bandit:/tmp/tempdir$ chmod +x script.sh
bandit24@bandit:/tmp/tempdir$ ./script.sh
bandit24@bandit:/tmp/tempdir$ grep 'Correct!' -n1 log.txt
5063-Wrong! Please enter the correct current password and pincode. Try again.
5064:Correct! # <-- code 5062
5065-The password of user bandit25 is [REDACTED] 
```

### Explanation
We're back to scripting, and it's going to need a lot of trial and error, both physically and literally. In this level we have to connect to a locally hosted daemon on port 30002, and give it the password for `bandit24` followed by a 4 digit passcode. Now if you'd like you can spend a couple hours inputting all possible combinations by hand, but since we can use scripts, we can do it automatically. Please note that this level is pretty advanced, and even I struggled a lot on my first time. If you are completely new to scripting, then chances you find your own solution are very low.

To start, we should make our own directory in `/tmp`. In here we will be making two files: `script.sh` and `log.txt`. `script.sh` will contain our script that we will run, and we will use `log.txt` to sift through all the responses we get back from the server. To edit the script, use `vi` which will open up an editor.

Quick theory since I skimmed over this last level, `vi` is a text editor in linux. It allows you to edit files line by line, and has a couple other features that we’ll be exploring next level. For now, know that it has two modes: a command mode, and an insert mode. Insert mode is where you can actually edit the text file, and command mode is where you can do certain actions. To switch to insert mode, use `i`, and to switch to command mode, which is on by default, simply use `esc`. 

Now in order to brute force this passcode, we have to check every combination, from 0000 to 9999. We can use the for loop from the last level, and include a set for it to iterate through, and it will do so. It will automatically apply the leading 0s if we type 0000. Now that we know `i` will iterate through every possible number within that range, we have to use `echo` with the variable at the end. Pretty simple.

By adding a pipe to the end of the for loop (`done`), we connect the `stdout` of the loop to the `stdin` of the daemon. Of course since we want the response of the daemon as well, we can use the redirection operator to append the response to a new line in the log file.

Remember that before we run the script, we have to give to make it executable, by using `chmod +x`. Once we run the script, our log file should be filled with responses from the daemon. I just searched for ‘Correct!’ using grep, however you can also exclude lines containing words, in this case incorrect. Either method works. By using the `-n1` flag, it also shows the lines below and above it. 

Now this level is really difficult, however it can be done. Just remember that if this is your first time, you are expected to use resources. You are expected to snoop around and try everything you can out. Don’t get demotivated, and remember to never give up. Below is the script I first used to solve this level, unnecessarily complicated, however since I didn’t know how anything worked, it’s what I came up with to solve the level. 

```bash
 #!/bin/bash

ad=0
bd=0
cd=0
dd=0

for ((i=0; i<=10000; i++)); do
        echo <bandit24 password> "${ad}${bd}${cd}${dd}"
        if ((dd<9));
        then
                ((dd++));
        else
                dd=0;
                ((cd++));
        fi
        if [ "$cd" -eq 10 ];
        then
                cd=0;
                ((bd++));
        fi
        if [ "$bd" -eq 10 ];
        then
                bd=0;
                ((ad++));
        fi
done | nc localhost 30002 >> log.txt
exit 
```

**Commands Learned:** `for` `chmod`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
