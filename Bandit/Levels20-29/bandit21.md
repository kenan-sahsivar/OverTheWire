# Bandit Level 20 → 21 Unfinished
## Level Goal
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

## Level 20 → 21 Solution
```bash
bandit20@bandit:~$ echo <password for bandit20> | nc -lp 11111
...
```
On _another_ terminal
```bash
bandit20@bandit:~$ ./suconnect 11111
```

### Explanation
Wow this one's difficult. Before anything, let me explain what a network daemon is. Well a daemon is essentially just a background process. You don't necessarily have access or control of it, it's something that runs on it's own, and does it's stuff in the background. A _network_ daemon is a background process that listens and responds to network requests and connections. Yes I know what you're thinking. SecureShell is NOT a network daemon, it is a protocol. However `sshd`, the process that runs in the background in charge of listening for client connections on port 22, _is_ a network daemon.

So essentially in this level, we're tasked with creating our own network daemon. This network daemon should send out the password for bandit20 when something is connected. We can create our own network daemon using netcat. The command for this is `nc`, and if we use the flag `-l` with it, which will listen for an incoming connection. We pair this with the `-p` flag to set our own port. Make sure that the port that you select doesn't conflict with another daemon. Since we want our network daemon to send out the password for bandit20, we should use echo <password> and pipe it, so that once another host connects, it will immediately send out the password. 

Once you have the network daemon set up, leave it open and start up another terminal window. On here, connect over to bandit20, and run the setuid. You should connect to whatever port 

**Commands Learned:** `nc` 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
