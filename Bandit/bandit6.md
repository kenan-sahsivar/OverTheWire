# Bandit Level 5 → 6
## Level Goal
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

    human-readable
    1033 bytes in size
    not executable

## Level 5 → 6 Solution
```bash
bandit5@bandit:~$ find inhere ! -executable -size 1033c
inhere/maybehere07/.file2
bandit5@bandit:~$ cat inhere/maybehere07/.file2
[REDACTED]
```

### Explanation
The file containing the password must be human-readable (filetype `ASCII text`), 1033 bytes in size, and it must not be executable. Although we should be starting by checking for human-readable files, we can actually find the solution without checking for filetype. In fact we don't even need to check if any files are executable, as simply checking for 1033 bytes will yield a single file. For the purpose of documentation, I will show how to do both.

First, as you've probably noticed, is that the solution contains 1033c. In bash, `1033b` would actually translate to 1033 blocks, not bytes. Blocks are a different unit of data than bytes, however it is irrelevant for the use of this current exercise. The correct use would be `1033c` which indicate *bytes*. So using the flag `-size`, we can specify `1033c` for 1033 bytes.

As for the `not executable` condition, we will make use of the `!` symbol. It has multiple use cases, however we will be using it as a logical NOT operator. As you may have already guessed, the `-executable` flag checks if the file is executable. By using `!` before it, we can check for files that *aren't* executable

**Commands Learned:** 

See [Byte](https://en.wikipedia.org/wiki/Byte) and [Block](https://en.wikipedia.org/wiki/Block_(data_storage))

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
