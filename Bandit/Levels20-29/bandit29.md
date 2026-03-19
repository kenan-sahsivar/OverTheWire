# Bandit Level 28 → 29
## Level Goal
There is a git repository at ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Level 28 → 29 Solution
```bash
user@machine:~$ git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
backend: gibson-0
bandit28-git@bandit.labs.overthewire.orgs password: 
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
user@machine:~$ ls repo
README.md
user@machine:~$ cat repo/README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
user@machine:~$ git -C repo log -p
commit b0354c7be30f500854c5fc971c57e9cbe632fef6
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 14 09:26:19 2025 +0000

    fix info leak

diff --git a/README.md b/README.md
index d4e3b74..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: [REDACTED]
+- password: xxxxxxxxxx
...
```

### Explanation
Same thing as the last level. Clone the repository onto your own machine. If you check inside of it, you'll see the credentials for the next level, however the password has been removed. Fortunately, we can check version history of a repository, to see what changes have been made to it. To do this, we can use the `git log` command. This will show us details about different _commits_, such as who made them, the dates, and descriptions about the changes. However, it won't show us what exactly the changes were.

To check the exact changes made, we can include the `-p` flag to include line changes as well. You'll see that there were a total of 3 commits made. The first commit was made by Ben Dover (interesting name), who just set up the file with the password written as '<TBD>'. The second commit was made by Morla Porla, who changed the password portion to the actual password of the next level. Finally, she made another commit where she essentially _censored_ the password. By checking the second commit, we can find the password for the next level.

Although optional, I also used the `-C` flag, which specifies a specific path to the repository. Since I didn't run `git log` in the directory of the repository, I simply wrote the location of the repository.

**Commands Learned:** `git-log`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
