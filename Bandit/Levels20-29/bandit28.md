# Bandit Level 27 → 28
## Level Goal
There is a git repository at ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo via the port 2220. The password for the user bandit27-git is the same as for the user bandit27.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Level 27 → 28 Solution
```bash
user@machine:~$ git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
Cloning into repo...
backend: gibson-0
bandit27-git@bandit.labs.overthewire.orgs password: 
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
user@machind:~$ ls repo
README
user@machine:~$ cat repo/README
The password to the next level is: [REDACTED]
```

### Explanation
Now we're learning how to use `git`. I'll do my best to provide a basic understanding of what `git` is and how it works.

Git in essence is just a software used for version control. Let's say you're working on a project, and you want to be able to do more than just save your work. Let's say you mess up and you break your code somewhere, however you don't know where. You would be able to revert whatever changes you made, you'd be able to the history of your changes, you'd be able to go back and check different versions of your code, and that's the power of Git. It's local, on your machine, and you're able to do as you please.

I'm sure you've heard of GitHub (hint: the site you're on), which is used for remote hosting. With GitHub, you and your friends could all be making changes on seperate machines to the same project, and merging your work together. If you want, anyone can view your repository, and anyone can push updates. That's the difference between Git and GitHub. 

Now for this machine, we have to use `git` on our own machine. You may need to install it, which you can check out [the official page](https://git-scm.com/install/) to do so. Once you have git installed, we can _pull_ the repository to our local machine. Notice that `git` uses `ssh` because we're essentially going into the user's machine and grabbing the files and copying it onto our machine. Since we're accessing the repo via port 2220, we have to include it after the hostname (bandit.labs.overthewire.org).

The repository will clone into the directory you're in, under the name of the repo (unless specified otherwise). You can check your active directory for the newly created directory. Check it and read the file inside for the password for the next level.

**Commands Learned:** `git`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
