# Bandit Level 8 → 9
## Level Goal
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

## Level 8 → 9 Solution
```bash
bandit8@bandit:~$ sort data.txt | uniq -u
[REDACTED]
```

### Explanation
In this level, we're introduced to 2 commands, and a new operator. I'll explain what both commands do first. The command `sort` essentially sorts the lines in a file. It is not required to know how exactly it sorts it, as in our case all we need is for identical lines to be adjacent to each other. The second command `uniq` can omit/remove repeated lines. The `-u` flag specifically does what we want, to only print unique lines. Now there's a catch, as the only way it knows if a line is repeated or not is if it is adjacent to it's duplicate. 

That's where the use of `sort` comes in. We first sort the file, then use `uniq -u` to omit repeated lines. However as you may have noticed there is a symbol in between both commands. The `|` is a control operator, and it's commonly referred to as a pipe. It takes the _stdout_ (standard output) of the previous command, and uses it as the _stdin_ (standard input) for the next command. You can chain pipes if you'd like to. 

If you recall from Level 6 → 7, we used the `2>/dev/null` redirector. There I explained that it was a redirector for _error_ messages. 
Now that you know the three default types of _data streams_, it's also usefull to know the file descriptors.

The `stderr` data stream is associated with the `2` file descriptor.

The `stdout` data stream is associated with the `1` file descriptor.

The `stdin` data stream is associated with the `0` file descriptor.

**Commands Learned:** `sort` `uniq`
See [file descriptors](https://en.wikipedia.org/wiki/File_descriptor) and [pipe](https://en.wikipedia.org/wiki/Pipeline_(Unix))

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
