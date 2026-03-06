# Bandit Level 7 → 8
## Level Goal
The password for the next level is stored in the file data.txt next to the word millionth

## Level 7 → 8 Solution
```bash
bandit7@bandit:~$ grep 'millionth' data.txt
millionth [REDACTED]
```

### Explanation
If you were to try to read data.txt, you'd quickly realize that it would be almost impossible to find the word 'millionth' in the file. That's what `grep` is for. `grep` is basically used to search for words or patterns in a file. When we specify a word, in this case 'millionth', it will return the line in which the word is located, which just so happens to be where our password is as well. 

**Commands Learned:** `grep`

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
