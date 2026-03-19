# Bandit Level 29 → 30
## Level Goal
There is a git repository at ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Level 29 → 30 Solution
```bash
user@machine:~$ git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
backend: gibson-0
bandit29-git@bandit.labs.overthewire.orgs password: 
remote: Enumerating objects: 16, done.
remote: Counting objects: 100% (16/16), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
user@machine:~$ cd repo
user@machine:~/repo$ ls
README.md
user@machine:~/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
user@machine:~/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
user@machine:~/repo$ git switch dev
branch 'dev' set up to track 'origin/dev'.
Switched to a new branch 'dev'
user@machine:~/repo$ ls
code  README.md
user@machine:~/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: [REDACTED] 
```

### Explanation
Same setup, clone the repository. This time the `README` says that there are no passwords in production. Weird. If you check the version history, there is no change related to the password either. However, repositories can also have multiple branches. Think of each branch as a different work station, where people can continue working on their own code without affecting the main branch. The main branch is often called the `master` or `main`. By using `git branch -a`, we check for all branches, both local and remote. 

We can switch over to different branches using `git checkout` or `git switch`. In the `dev` branch, the password to the next level is stored in the `README.md` file.
**Commands Learned:** `git-switch`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
