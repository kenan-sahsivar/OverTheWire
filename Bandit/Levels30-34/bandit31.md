# Bandit Level 30 → 31
## Level Goal
There is a git repository at ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Level 30 → 31 Solution
```bash
user@machine:~$ git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
backend: gibson-0
bandit30-git@bandit.labs.overthewire.orgs password: 
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
user@machine:~$ cd repo
user@machine:~/repo$ ls
README.md
user@machine:~/repo$ cat README.md
just an epmty file... muahaha
user@machine:~/repo$ git tag
secret
user@machine:~/repo$ git show secret
[REDACTED]
```

### Explanation
Once again, working with `git`. I'm starting to think the rest of the levels will be like this. In this one, there is nothing useful in the `README.md`. There are no other commits, and no other branches we can use to figure out the password. What we _can_ do though is check the tags. In short, tags are like labels that mark important points in commits. For example you might create a tag for a specific release version. In fact many people do this. 

To check all tags, you can use `git tag`. To show more information about a tag, you can use `git show`. In this case, the only information provided is the password for the next level. Works for me. 

**Commands Learned:** `git-tag` `git-show`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
