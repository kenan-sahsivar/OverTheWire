# Bandit Level 31 → 32
## Level Goal
There is a git repository at ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.

From your local machine (not the OverTheWire machine!), clone the repository and find the password for the next level. This needs git installed locally on your machine.

## Level 31 → 32 Solution
```bash
user@machine:~$ git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo
backend: gibson-0
bandit31-git@bandit.labs.overthewire.orgs password: 
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
user@machine:~$ cd repo
user@machine:~/repo$ ls -a
.  ..  .git  .gitignore  README.md
user@machine:~/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master

user@machine:~/repo$ cat .gitignore
*.txt
user@machine:~/repo$ rm .gitignore
user@machine:~/repo$ echo 'May I come in?' > key.txt
user@machine:~/repo$ git add ,
user@machine:~/repo$ git commit -m "My Commit"
[master 5aa652f] My Commit
 2 files changed, 1 insertion(+), 1 deletion(-)
 delete mode 100644 .gitignore
 create mode 100644 key.txt
user@machine:~/repo$ git push
backend: gibson-1
bandit31-git@bandit.labs.overthewire.orgs password: 
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 293 bytes | 293.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: [REDACTED] 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
```

### Explanation
Cutting to the chase, this time we are tasked with pushing a file to the remote repository. Remember, the remote repository is not on our system, it is the one hosted by bandit31-git@bandit.labs.overthewire.org. Specifically, we have to create a file called `key.txt` with the content `'May I come in?'`. Now I haven't shown it in the solution, but if you tried to create this file without removing or editing the `.gitignore` file, there would be no change. You would not be able to push the file to the remote repository because `git` is ignoring all files ending with `.txt`. You can use `.gitignore` to ignore unnecessary files that solely take up space or are redundant when pulling or pushing files. 

Now after deleting `.ignore`, once we create the `key.txt` file, you can check `git status` to see the changes you've made so far. Once you're satisfied, use `git add` to prepare the changes to the next commit. By using `.` as the file path, we specify our current working directory. Once you've done so, now you can actually commit your changes, using `git commit`. You can use the `-m` flag to specify a message, like a title, or just ignore it and input it afterward in a text editor. 

Once we've made our commit, now it's time to push it. Using `git push`, we connect back to the remote repository and push our changes. If we've made the `key.txt` file properly, the remote server should send back a message with the password to the next level. Now if I'm going to be honest, I have no idea how it sends back a message and validates the changes, but it does what it does. I believe this level is the last of the `git` levels. 

**Commands Learned:** `git-add` `git-commit` `git-push`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
