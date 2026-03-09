# Bandit Level 15 → 16
## Level Goal
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Level 15 → 16 Solution
```bash
bandit15@bandit:~$ openssl s_client -port 30001
Certificate stuff...
---
read R BLOCK
(Level 14-15 Password) # <-- input here
Correct!
[Redacted]
```

### Explanation

For this level, the objective is similar, however the connection has to be secure. Since `telnet` isn't secure, we have to use a different protocol. A program we can use for this is `openssl` which is responsible for many of the things you can do on the internet. In short, it's a library that people use for things like encryption and connection. 

Since we have to use SSL/TLS encryption, we can use the `s_client` command, which acts as an SSL/TLS client to establish secure connections. We will be connecting to port 30001, so the command will look something like this: `openssl s_client -port 30001`/ Remember that in order to use localhost, we have to already be connected to the system; you may continue off from bandit15. 

Once we've established a connection, the port will be listening for a password, specifically the one we got from the last level. If it is correct, it will spit back out the password for the next level.

**Important Note:** If you receive a message like "DONE" or "RENEGOTIATING", simply include `-quiet` as a flag. You may check out the next level as to why this happens, as I don't want to explain the same concept twice. 

**Commands Learned:** `openssl` `s_client`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
