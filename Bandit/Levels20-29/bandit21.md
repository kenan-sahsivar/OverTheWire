# Bandit Level 20 → 21
## Level Goal
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think

## Level 20 → 21 Solution
```bash
bandit20@bandit:~$ echo <password for bandit20> | nc -lp 11111
[REDACTED]
...
```
On _another_ terminal
```bash
bandit20@bandit:~$ ./suconnect 11111
Read: <password for bandit20>
Password matches, sending next password
```

### Explanation
Wow this one's difficult. Before anything, let me explain what a network daemon is. Well a daemon is essentially just a background process. You don't necessarily have access or control of it, it's something that runs on it's own, and does it's stuff in the background. A _network_ daemon is a background process that listens and responds to network requests and connections. Yes I know what you're thinking. SecureShell is NOT a network daemon, it is a protocol. However `sshd`, the process that runs in the background in charge of listening for client connections on port 22, _is_ a network daemon.

So essentially in this level, we're tasked with creating our own network daemon. This network daemon should send out the password for bandit20 when something is connected. We can create our own network daemon using netcat. The command for this is `nc`, and if we use the flag `-l` with it, which will listen for an incoming connection. We pair this with the `-p` flag to set our own port. Make sure that the port that you select doesn't conflict with another daemon. Since we want our network daemon to send out the password for bandit20, we should use echo <password> and pipe it, so that once another host connects, it will immediately send out the password. 

Once you have the network daemon set up, leave it open and start up another terminal window. On the new window, connect over to bandit20, and run the setuid. You should connect to whatever port you set, and upon connection, the password to the next level should appear on the terminal which has the network daemon running.

**Commands Learned:** `nc` 

### Exercise
The idea of a network daemon may not have quite clicked with you yet, and that's perfectly fine. Here's an exercise that can help visualize how exactly this level works. First, open up a terminal and run the command: `echo 'testing, hello world' | nc -lp 11111`. Note that users without root priveleges cannot bind to ports below 1024. This is because they are well known ports (privileged ports), and restricting them avoids conflict. What our command does, is that we send out (echo) the message 'testing, hello world' to `port 11111` when someone connects. Until then, netcat is simply listening. We'll call this terminal our _listening terminal_.

On another terminal, connect to your network daemon by using `telnet localhost 11111`. You may also use `nc localhost 11111`. Remember that since the daemon is on our machine, we connect using localhost. If you remember, telnet was a way to connect to processes, however it is unsecure since there is no encryption. Upon connection, it'll say something like `Connected to localhost.` and then right afterward, you'll see `testing, hello world`. We'll call this terminal our _connected terminal_.

Now you might notice that anything you type on the _connected terminal_ will appear on the _listening terminal_. That's essentially how the level works. Once the _listening terminal_ sends out the message for the _connected terminal_ to see (./suconnect), it responds with the password which is visible on our _listening terminal_. Note that the vice versa will not work, as we have used `echo` for the _listening port_. Remember that the `|` (pipe) will bind the stdout of `echo` to the `stdin` of nc. Once `echo` has sent out the message, it's job is done, so it closes. Once `echo` closes, it stops sending out data, essentially closing it's mouth. So although it's ears can still listen for messages, it no longer has a mouth. It must scream. Hopefully this helped.

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
