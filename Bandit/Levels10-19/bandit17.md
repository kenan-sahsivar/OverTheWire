# Bandit Level 16 → 17
## Level Goal
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

Helpful note: Getting “DONE”, “RENEGOTIATING” or “KEYUPDATE”? Read the “CONNECTED COMMANDS” section in the manpage.

## Level 16 → 17 Solution
```bash
bandit16@bandit:~$ nmap localhost -p 31000-32000
Nmap things...
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
bandit16@bandit:~$ openssl s_client -port 31790 -quiet
s_client things...
(Level 15-16 Password) # <-- input here
Correct!
[RSA PRIVATE KEY]
```

### Explanation
Once again, since the level specificies that we must connect through localhost, we have to already be connected to the machine. Once we're in, we will be doing what's called a _port scan_. You might be asking what a port scan is, and why it's important. Well in short, a port scan can check what ports are open on a machine, and using this information we can connect to the machine. For example, if you didn't know that `bandit.labs.overthewire.org` was open on port 2220, we wouldn't be able to connect to the server, unless we used a tool like `nmap` to check for the open ports. 

Using `nmap` is pretty straight forward, and all we have to do is specify the ip/dns (localhost in our case), and specify the ports we want to search. We'll then get back a list of ports that are either open or filtered, and `nmap` will automatically filter out the closed ones. In this case we got back 5 open ports.

The level tells us that we must use the one that speaks SSL/TSL. If you remember from last level, that's the one we used `openssl s_client` with. There really isn't a better way to do this (correct me if I'm wrong), but we will have to do the guess and check method for these 5 ports. I'll save you some time, and let you know that port 31790 is the correct one. Once we figure out which port is the right one, we connect using `openssl s_client` and specify the port number. 

If you've noticed, I've also used the `-quiet` flag. When communicating with a port that uses TLS, there are specific keywords that you can use that will perform different tasks. For example, if you input the letter `k`, then the port will send back `KEYUPDATE`. You don't need to know what this is for, and I'm not exactly sure what this is used for either, however just know that since the password we got from 15-16 starts with the letter 'k', it will throw a `KEYUPDATE`. To avoid this, we use the `-quiet` flag so that the port stops listening for these characters. 

Once you receive the private key, remember to copy it and save it on your machine so that you can use it for the next level. 

**Commands Learned:** `nmap`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
