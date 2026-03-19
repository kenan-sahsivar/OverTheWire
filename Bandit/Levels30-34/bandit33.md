# Bandit Level 32 → 33
## Level Goal
After all this git stuff, it’s time for another escape. Good luck!

## Level 32 → 33 Solution
```bash
user@machine~:/ ssh bandit32@bandit.labs.overthewire.org -p 2220
backend: gibson-1
bandit32@bandit.labs.overthewire.orgs password
WELCOME TO THE UPPERCASE SHELL
>> $0
$ ls -l
-rwsr-x--- 1 bandit33 bandit32 15140 Oct 14 09:26 uppershell
$ whoami
bandit33
$ cat /etc/bandit_pass/bandit33
[REDACTED]
```

### Explanation
Last level. We're finally back to using `ssh`, and once we do establish a connection, it's clear something is wrong. We are not in any normal shell, we are in the "UPPERCASE SHELL", which capitalizes all inputs. This poses a problem since we're not able to use almost any command. 

There are a couple things you can try. For example, `$HOME` works, so does `$PWD`, etc. These are called environment variables. They'll return their values, however they're just variables, they don't help us in any way. There is one thing that can help us though, and before I talk about it, I should first explain why it would work.

If you check `/etc/passwd` from another user, you'll see that `bandit23` uses a file as a shell. If you check the type of file, you'll see that it's a setuid binary. If you remember, these are executable files that run with the permissions of the owner. If you check the permissions, you'll see that `bandit32` isn't the owner of this file, `bandit33` is. We can verify this later. However right now, we know that uppershell isn't an actual shell, it's just a binary executable. If anything, we're probably stuck in a loop that looks like this[^1].

```
while(true) {
  print(">> ");
  cmd = makeUppercase(readInput());
  print(execute("sh", "-c", cmd));
}
```

The shell is still `sh`, but it's being passed through this executable and turning all of our commands into uppercase. Can we do anything about it? Why yes we can. There's a _variable_ we can use called `$0`, which gives the path to the shell currently being used. I'm using the term variable, however it's important to note that it should be classified as a _special parameter_. Differences? A variable is a type of parameter which you can assign a name and value to. A special parameter is predefined by the shell, and usually can't be assigned by the user. Going back to `$0`, since the file where the shell resides in is an executable, running `$0` will take us to an actual usable shell. 

If you want, you can test this out on your own machine. If you use `$0`, it will "reload" the shell with its configuration (.zshrc for me), and if you use `echo` with it, you'll see the path name. This should show you that it's being executed and loading the shell. 

Going back to the workings of the binary executable, since it's owned by `bandit33`, we now have the power of `bandit33` in our hands. Obviously we can now simply grab the password from `etc/bandit_pass/bandit33`. 

Note that this is the final level, so you can't do much with this password. 

**Commands Learned:** 

[^1]: [reddit explanation](https://www.reddit.com/r/hacking/comments/dxe2c2/comment/f7pl4j0/)

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
