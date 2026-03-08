# Bandit Level 12 → 13
## Level Goal
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

## Level 12 → 13 Solution
```bash
bandit12@bandit:~$ mktemp -d
/tmp/mytempdir
bandit12@bandit:~$ cd /tmp/mytempdir
bandit12@bandit:/tmp/mytempdir$ xxd -r ~/data.txt > start.bin
bandit12@bandit:/tmp/mytempdir$ file start.bin
gzip compressed data...
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | file -
bzip2 compressed data...
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | file -
gzip compressed data...
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | file -
POSIX tar archive (GNU)
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | tar -xO | file -
POSIX tar archive (GNU)
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | tar -xO | tar -xO | file -
bzip2 compressed data...
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | tar -xO | tar -xO | bunzip2 | file -
POSIX tar archive (GNU)
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | tar -xO | tar -xO | bunzip2 | zcat | file -
ASCII text
bandit12@bandit:/tmp/mytempdir$ zcat start.bin | bunzip2 | zcat | tar -xO | tar -xO | bunzip2 | zcat | cat
[REDACTED]
```
ALTERNATIVELY (shortcut)
```bash
bandit12@bandit:~$ mkdir /tmp/mytempdir
/tmp/mytempdir
bandit12@bandit:~$ xxd -r data.txt > /tmp/mytempdir/start.bin
bandit12@bandit:~$ zcat /tmp/mytempdir/start.bin | bunzip2 | zcat | tar -xO | tar -xO | bunzip2 | zcat | cat
```
### Explanation

This level is a mess of compression, and the way we solve it is similar to that of a [Matryoshka Doll](https://en.wikipedia.org/wiki/Matryoshka_doll). There are several nesting compressions, and we need to know how to decompress each level.

Before we start, we must first create a directory in the `/tmp` directory. Bandit allows us to do so using `mktemp -d`, creating a temporary directory with a randomly generated name.

Now before we get into any decoding, we will change directories to the one that we just created. We do this by using `cd`. The directory is currently empty, so we can start our solution by undoing the hex dump and moving it to our directory. A hexdump in a nutshell displays binary data in hexadecimal format. You can create your own hexdump using the `xxd` command, which can create a hexdump of a file you specify. 

Using this command, you can also undo a hexdump, or revert it. To do this we use the `-r` flag, which will revert a hexdump. Now simply using `xxd -r` will display the output directly in the shell. Since we want to keep working with it, it's best that we move it into a file. This is where we use the `>` operator, which if you remember, will redirect it wherever we want. I chose `/tmp/mytempdir/start.bin`. Even though we never explicitly created `start.bin`, it is automatically created when we redirect the *stdout*. 

The rest of this solution gets a little complicated. The next step is to figure out what type of of file our new `start.bin` is. We can use `file` for this. Since we start off with a `gzip` compressed file, we can use the `zcat` command to decompress the file. Since we'll need to know what type of file we're working with next, we can simply use `|` (pipe) to move the _stdout_ of `zcat` into the _stdin_. Using a hyphen `-` will default some commands to read from the _stdin_, and in this case using `file -` will allow us to directly check the filetype of whatever we've just decoded. 

Instead of creating several new files for every time that we decompress something, it's optimal to instead set up a line of commands using pipe, and reading what the next step is by checking the filetype after every step. If you see that a file is compressed using `bzip2`, you can use `bunzip2` to decompress it. Alternatively, if it has been compressed using `gzip`, you may use `zcat`. 

Lastly, `tar` is an archiving utility, meaning it basically just bundles up multiple files and directories into one single file. To extract the files that are archived, we can use the `-x` flag. This by default will extract the files into the active directory. Since we want to be able to check the filetype of the file that is extracted, we should use the `-O` flag to extract the files to the _stdout_ where we can use `|` and check for the filetype using `|`.

This level tests your skills with decompression and maintaining file structure, so it's fine if you feel a little challenged. During this stage you should start getting used to using the pipe operator, as well as directories and file types. 

**Commands Learned:** `mktemp` `xxd` `gzip` `zcat` `bzip2` `bunzip2` `tar` 

---
### Disclaimer
I do not own the rights to OverTheWire or its wargames. These write-ups are for my own educational documentation and to share methodology, not to provide shortcuts. Support the community at [OverTheWire.org](https://overthewire.org).
