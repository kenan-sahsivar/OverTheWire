# Bandit Level 14 → 15
## Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

## Level 14 → 15 Solution
```bash
bandit14@bandit:~$ telnet localhost 30000
Connected to localhost.
(Level 13-14 Password)
Correct!
[Redacted]
```

### Explanation

We're making a big shift into localhost. If you don't already know, localhost represents your own computer in terms of networking, and you can use it to access local services. Since the computer is communicating with itself, you must already be connected to the system to use localhost. This is why we have to start off the level with bandit14, or really any level. As long as we're connected to the system we should be able to use localhost. 

The command we'll be using is `telnet`, which is basically an ancient method for network protocol, similar to secure shell. One of the biggest differences is that it's widely recognized as unsecure, since it does not use encryption. For our purposes, it works fine, and we can use `telnet localhost 30000` to connect to the port specified. Once in, the port is listening, waiting for a password from us. If correct, it will give us the password for the next level. The password it expects is the one used to get into bandit14.

**Commands Learned:** `telnet`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
