# Bandit Level 0
## Level Goal
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

## Level 0 Solution
`ssh bandit.labs.overthewire.org -p 2220 -l bandit0`

**ALTERNATIVELY**

`ssh bandit0@bandit.labs.overthewire.org -p 2220`

From here simply type in the password provided, being `bandit0`.

### Explanation
The aim of the first level is to teach the player a crucial command, being `ssh`. SSH or Secure Shell is a protocol used to securely and remotely connect to and manage computers over unsecured networks like the internet. The host we are trying to connect to would be bandit.labs.overthewire.org. 

We can use the open port 2220 provided to connect to the interactive environment, by using the option (or Flag) -p for port, and 2220 (argument) to specify the port number. This port is left open for us to connect to, and we will be using it for the majority of the up and coming levels. 

The option -l allows us to specify the user who we would like to connect as. Since the level goal requires us to log in to bandit0, we should use bandit0 as the argument to this option. Alternatively, we can use bandit0@bandit.labs.overthewire.org as the positional argument (main argument) to avoid using an extra opton.


**Commands Learned: ssh**

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
