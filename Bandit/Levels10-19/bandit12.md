# Bandit Level 11 → 12
## Level Goal
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

## Level 11 → 12 Solution
```bash
bandit11@bandit:~$cat data.txt | tr [a-zA-Z] [n-za-mN-ZA-M]
[REDACTED]
```

### Explanation
We're now starting to dive into more complex commands. The task is to decode the password, using what we know about the encryption. The level description says that the letters have been rotated by 13 positions. This method of encoding is known more commonly as ROT13 (Rotate 13). Its a cipher, or more commonly known caesar cipher. 

The way we'll be doing this is by using the `tr` (translate) command. It allows us to set specifications on what exactly we're translating, and what we're translating it to. It'll make sense soon.

To understand the way `tr` will work, we have to first understand what exactly ROT13 is. ROT13 is an encryption method which shifts each letter toward the right (in the alphabet) 13 times. For example, If I start off with A (1st letter in the alphabet), I would end up with N (14th letter in the alphabet). There has been an increment of 13. So logically if we want to apply this, we have to first specify the target letters (original character set), and what they are being translated to (shifted character set). 

For example if I wanted to change all lower case letters to upper case, my first set would be [a-z] and my changed set would be [A-Z]. If I wanted to only affect a certain grouping of letters, I could use something like [t-z] [T-Z]. In fact I can even change the letter order, which is what we're about to do. 

Starting off with just lowercase letters, we know that our original character set should be [a-z]. If we want a ROT13, 'a' would have to become 'n'. Since we're shifting in a loop, z would become the 13th letter, or 'm'. So applying that change, we should arrive to something like [a-z] [n-m]. However you'll realize that the letters are in reverse order (n comes after m), so this obviously won't do. This is why we seperate the second set into two distinct ranges.

Our original is still [a-z], but our changed becomes [n-z + a-m]. The shell doesn't allow for this format though, and just wants us to keep everything together. For lowercase, the `tr` command would look like `tr [a-z] [n-za-m]`. If we also want to incorporate upper cases, we can add it just as easily. The final command looks like `tr [a-zA-Z] [n-za-mN-ZA-M].

**Commands Learned:** `tr`

*Note: if you're running the `tr` command on your local machine and you happen to be using a shell like _zsh_, the correct use may be `tr '[set1]' '[set2]'`*

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
