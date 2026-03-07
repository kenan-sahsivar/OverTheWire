# Bandit Level 10 → 11
## Level Goal
The password for the next level is stored in the file data.txt, which contains base64 encoded data

## Level 10 → 11 Solution
```bash
bandit10@bandit:~$base64 -d data.txt
[REDACTED]
```

### Explanation
This level is quite simple. Base64 is used for encoding ASCII text into binary. See the diagram below for a basic understanding of how base64 works. In this level all we have to do is decode the file. To do this, we'll use `base64 -d`, the `-d` flag being used to _decode_. 

<img width="412" height="281" alt="image" src="https://github.com/user-attachments/assets/eb18a918-e1aa-4ac7-96b3-629358c44ccf" />

**Commands Learned:** `base64`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
