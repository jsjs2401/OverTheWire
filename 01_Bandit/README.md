# Introduction

[Bandit](https://overthewire.org/wargames/bandit/ "Landing page for Bandit wargame") serves as an introduction to basic Linux, and to Git in the later levels.

Windows' command prompt was used for logging in to each level.

# Overview

| Level | Concepts Needed                                             | New Commands Used                                               |
| :---: | :---------------------------------------------------------- | :-------------------------------------------------------------- |
| 0     | SSH login                                                   | ssh                                                             |
| 1     | Reading files                                               | ls, cat                                                         |
| 2     | Accessing files with special characters                     |                                                                 |
| 3     | Accessing files with spaces in filename                     |                                                                 |
| 4     | Changing directories, accessing hidden files                | cd                                                              |
| 5     | File types                                                  | file                                                            |
| 6     | File sizes, non-executable files, grep and piping           | du, find, grep, \|                                              |
| 7     | File user and file group ownership                          |                                                                 |
| 8     | Grep and piping of file contents                            |                                                                 |
| 9     | Sorting and finding unique values                           | sort, uniq                                                      |
| 10    | Finding specific strings in a file                          | strings                                                         |
| 11    | Base64 encoding and decoding                                | base64                                                          |
| 12    | Rot13 substitution cipher                                   | tr                                                              |
| 13    | Hexdump, compression/decompression and file signatures      | mkdir, mktemp, cp, mv, xxd, gzip, bzip2, tar, rm                |
| 14    | SSH login with private key                                  |                                                                 |
| 15    | Netcat and network communication                            | nc                                                              |
| 16    | SSL encryption                                              | openssl s_client                                                |
| 17    | Port scanning, service detection, file permissions          | nmap, chmod                                                     |
| 18    | Finding differences between files                           | diff                                                            |
| 19    | SSH remote command execution, shells, bash scripting        |                                                                 |
| 20    | Set user ID (setuid), file permissions                      |                                                                 |
| 21    | Background processes, network daemons, setuid binary        |                                                                 |
| 22    | Cronjobs, bash scripting                                    |                                                                 |
| 23    | Cronjobs, bash scripting, bash variables                    | whoami, md5sum, cut                                             |
| 24    | Cronjobs, bash scripting, bash variables, file permissions  | stat, timeout, nano                                             |
| 25    | Brute forcing passwords, bash scripting, netcat             |                                                                 |
| 26    | Shell environments, vim                                     | more, set                                                       |
| 27    | Setuid binary                                               |                                                                 |
| 28    | Git introduction                                            | git clone                                                       |
| 29    | Git commit history                                          | git log, git show                                               |
| 30    | Git branching                                               | git branch, git switch, git checkout                            |
| 31    | Git tagging                                                 | git tag                                                         |
| 32    | Git add, commit, push, .gitignore                           | git add, git commit, git diff, git reset, git restore, git push |
| 33    | Bash special variables, shell environments                  |                                                                 |

# Contents

- [Level 0](#level-0)
- [Level 1](#level-1)
- [Level 2](#level-2)
- [Level 3](#level-3)
- [Level 4](#level-4)
- [Level 5](#level-5)
- [Level 6](#level-6)
- [Level 7](#level-7)
- [Level 8](#level-8)
- [Level 9](#level-9)
- [Level 10](#level-10)
- [Level 11](#level-11)
- [Level 12](#level-12)
- [Level 13](#level-13)
- [Level 14](#level-14)
- [Level 15](#level-15)
- [Level 16](#level-16)
- [Level 17](#level-17)
- [Level 18](#level-18)
- [Level 19](#level-19)
- [Level 20](#level-20)
- [Level 21](#level-21)
- [Level 22](#level-22)
- [Level 23](#level-23)
- [Level 24](#level-24)
- [Level 25](#level-25)
- [Level 26](#level-26)
- [Level 27](#level-27)
- [Level 28](#level-28)
- [Level 29](#level-29)
- [Level 30](#level-30)
- [Level 31](#level-31)
- [Level 32](#level-32)
- [Level 33](#level-33)
- [Level 34](#level-34)

---

# Level 0

## Task

Log in to `bandit.labs.overthewire.org` on port `2220` via SSH. The username is `bandit0` and the password is `bandit0`.

## Solution

The syntax for `ssh` follows `ssh <username>@<hostname> -p <port>`.

Without the `-p <port>` flag, it defaults to port 22.

Entering the command without specifying the port, `ssh bandit0@bandit.labs.overthewire.org`, gives the following:

```
jsjs2401>ssh bandit0@bandit.labs.overthewire.org
[...]
                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server on port 22, which is not intended.

bandit0@bandit.labs.overthewire.org: Permission denied (publickey).
```

The proper command would instead be:

`ssh bandit0@bandit.labs.overthewire.org -p 2220`

Entering this brings me to a prompt for the password, bandit0, which when keyed in would log me in to the remote machine.

---

# Level 1

## Task

The password for the next level is stored in a file called `readme` located in bandit0's home directory. Use this password to log into `bandit1` using SSH, using the same port. All subsequent levels also use port `2220`.

## Solution

Two new commands here: `ls` and `cat`.

`ls <path>` lists the contents in the specified path. Without a path argument, it defaults to the current directory.

`cat <file>` displays the contents of the file.

```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walkthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!

The password you are looking for is: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

---

# Level 2

## Task

The password for the next level is stored in a file called `-` located in the home directory.

## Solution

Logging in and entering `ls`, it shows a file named `-`. Trying to read the file using `cat -` leads to a situation where anything that is user-entered is echoed back. Ctrl+C is used to exit the program.

```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat -
a
a
^C
```

Linux syntax treats `-` as a special symbol, typically used for command flags as seen with `-p` when using `ssh bandit0@bandit.labs.overthewire.org -p 2220` earlier. It is hence not recommended to have `-` as the first character of file or directory names.

For `cat`, the [Linux man pages](https://www.man7.org/linux/man-pages/man1/cat.1.html "Linux manual page for cat command") state that "when FILE is -, read standard input".

To avoid having the file name intepreted as a special symbol, the directory path has to be specified.
- `.` refers to the current directory
- `..` refers to the parent directory
- `~` refers to the home directory, defined for each user.

```
bandit1@bandit:~$ cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

---

# Level 3

## Task

The password for the next level is stored in a file called `spaces in this filename` located in the home directory.

## Solution

Similar to the previous level, it is not recommended to include spaces in the filename, as spaces typically indicate separate arguments for the command.

`cat` will attempt to parse the command as opening four files of `spaces`, `in`, `this`, `filename`.

```
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

In this case, the solution would be to enclose the filename in single or double quotes.

```
bandit2@bandit:~$ cat "spaces in this filename"
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

---

# Level 4

## Task

The password for the next level is stored in a hidden file in the `inhere` directory.

## Solution

`cd <path>` is used to change the working directory. The path given can be relative (e.g. `cd inhere`, `cd ..`), or absolute (e.g. `cd /home/bandit2`).

In addition to `cd ~` going to the home directory, `cd /` goes to the root directory.

```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere
bandit3@bandit:~/inhere$ ls
```

`ls` shows nothing when used in the `inhere` directory. In Linux, files starting with `.` are hidden, and ls only shows non-hidden files by default.

Consulting the [Linux man pages](https://man7.org/linux/man-pages/man1/ls.1.html "Linux manual page for ls command") for `ls`, or entering `man ls`, we find two flags of interest: `-a` and `-A`.

`-a` shows all files and directories, including those starting with `.`, while `-A` shows all files and directories but ignores `.` and `..`—the current and parent directories respectively.

```
bandit3@bandit:~/inhere$ ls -a
.  ..  ...Hiding-From-You
bandit3@bandit:~/inhere$ ls -A
...Hiding-From-You
bandit3@bandit:~/inhere$ cat ...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

## Extras

The file could also be accessed without leaving the home directory, were the full path known:

```
bandit3@bandit:~$ cat inhere/...Hiding-From-You
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

---

# Level 5

## Task

The password for the next level is stored in the only human-readable file in the `inhere` directory. Tip: if your terminal is messed up, try the `reset` command.

## Solution

There are several files in the directory, and the password could be found using dumb brute force by repeatedly entering `cat ./-fileXX`.

```
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls -A
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ cat ./-file00
�ŉOT���S �plS]-EH�t�:-�Z�
bandit4@bandit:~/inhere$ cat ./-file01
N$���'���Se��
             \�- V�P�jls�����
```

This is inefficient, and we could instead use `file <filename>` to determine the file type. It is possible to scan many files using the `*` wildcard character, either using `file ./*` or `file ./-file*`.

```
bandit4@bandit:~/inhere$ file ./*
./-file00: PGP Secret Sub-key -
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```

Human-readable formats are typically either ASCII text or Unicode, and the only file matching the requirements is `-file07`. Opening it reveals the password.

```
bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

## Extras

The password could conceivably also be found using `cat ./*`, but it produces a mess and is infeasible when there are many more files to scan through.

```
bandit4@bandit:~/inhere$ cat ./*
�ŉOT���S �plS]-EH�t�:-�Z�
                         N$���'���Se��
                                      \�- V�P�jls�����
                                                      o5e�Mz9�#P�ws������Oh||xt��6|ر��Vܒ��q ��*rMӼ^';b\�
x����]C�
        �H`�/�X��OGLV��*��-o��w9�P�RAz�b��␦[��F���_��+J��2X1�M�O�g��Y����d�Ŧj4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
t)�r�R�C#�ӧ��4��_�\����^�)C
```

---

# Level 6

## Task

The password for the next level is stored in a file somewhere under the `inhere` directory and has all of the following properties:

- Human-readable.
- 1033 bytes in size.
- Not executable.

# Solution

```
bandit5@bandit:~$ cd inhere
bandit5@bandit:~/inhere$ ls -A
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
```

Each of the `maybehere`s are directories containing files. It would be possible to use `file */{.,}*` to return the details of all contained files, but many of them are human-readable.

The purpose of the curly braces are to take advantage of [brace expansion](https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html "Brace expansion page from Bash reference manual"), where `{.,}` indicates to look for cases where there is either `.` or nothing between `/` and `*`.

```
bandit5@bandit:~/inhere$ file maybehere*/*
maybehere00/-file1:       ASCII text, with very long lines (1038)
maybehere00/-file2:       ASCII text, with very long lines (9387)
maybehere00/-file3:       OpenPGP Secret Key
maybehere00/spaces file1: ASCII text, with very long lines (6117)
maybehere00/spaces file2: ASCII text, with very long lines (6849)
maybehere00/spaces file3: data
[...]
```

`du` could be used to show the file sizes, with the flags `-a` to scan all files, and `-b` to give the file size in bytes.

The output could then be sent to another command, `grep <pattern>`, which is used to match lines that match a given pattern.

The output is sent to another command using the pipe `|`. Its syntax is `<command1> | <command2>` where the output of `command1` is piped as input to `command2`.

```
bandit5@bandit:~/inhere$ du -ab | grep 1033
1033    ./maybehere07/.file2
bandit5@bandit:~/inhere$ cat maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

We are lucky that only one file is 1033 bytes, and opening it gives the password.

## Extras

`ls -l` also gives the file size, and could have also been used with `grep` to find the correct file.

The syntax for the output of `ls -l` is, in the column order:
1. Ten letters and dashes showing the file type and file permissions. More details in later levels.
2. The number of hard links to the file or directory. This is usually one for a file, and two for a directory since it has a link to itself, `.`, and a link from its parent directory.
3. Name of user who owns the file.
4. Name of group which owns the file.
5. File size in bytes.
6. Date of last modification.
7. File name.

```
bandit5@bandit:~/inhere$ ls -l */{.,}* | grep 1033
-rw-r----- 1 root bandit5 1033 Apr 10 14:23 maybehere07/.file2
```

Alternatively, `find` has several flags that allow it to search for all the conditions simultaneously.

- `-size 1033c` where the `c` indicates that it will be looking for 1033 bytes, instead of the default where it looks for 512-byte blocks. This flag will be omitted from the subsequent demonstrations.
- `! -executable` where `-executable` searches for executable files, and the results are negated by `!` so the final results are all non-executable files.
- `-type f` specifies that `find` only searches for files and not directories

```
bandit5@bandit:~/inhere$ find ! -executable -type f
./maybehere00/spaces file2
./maybehere00/-file2
./maybehere00/.file2
./maybehere13/spaces file2
./maybehere13/-file2
[...]
```

`find` also has an flag to execute a command on each file that matches, `-exec`. More details can be found in the [Linux man pages for `find](https://man7.org/linux/man-pages/man1/find.1.html#EXAMPLES "Linux manual page for find command").

This flag can be used as such: `find -exec file '{}' \;` to run `file` on each match, which can then be piped into `grep ASCII`.

```
bandit5@bandit:~/inhere$ find ! -executable -type f -exec file '{}' \; | grep ASCII
./maybehere00/spaces file2: ASCII text, with very long lines (6849)
./maybehere00/-file2: ASCII text, with very long lines (9387)
./maybehere00/.file2: ASCII text, with very long lines (7835)
./maybehere13/spaces file2: ASCII text, with very long lines (951)
./maybehere13/-file2: ASCII text, with very long lines (1422)
[...]
```

Ultimately, the file size is still needed to find the exact file needed.

---

# Level 7

## Task

The password for the next level is stored **somewhere on the server** and has all of the following properties:

- Owned by user bandit7.
- Owned by group bandit6.
- 33 bytes in size.

## Solution

Now we'll need to search elsewhere on the server, not just in the home directory.

`find` also has additional flags `-user` and `-group` to search for the users and groups who own the file.

As before, the flags `-type f` and `-size 33c` are also used.

```
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c
find: ‘/root’: Permission denied
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/1042589/task/1042589/fdinfo/6’: No such file or directory
[...]
/var/lib/dpkg/info/bandit7.password
[...]
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

## Extras

A lot of errors appeared in the results as the user bandit6 did not have permission to access the files. One way to hide this is to use `2>/dev/null`.

- `> file` is an operator that directs the output (stdout) to write to `file`. This also overwrites the `file`.
- `>> file` is an operator that appends the output to existing data already in `file`.
- `1> file` directs stdout to `file`.
- `2> file` directs error outputs (stderr) to `file`.
- `/dev/null` is a null device that throws away any input given to it

Put together, `2>/dev/null` bins any error message outputs, effectively suppressing them.

```
bandit6@bandit:~$ find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

---

# Level 8

## Task

The password for the next level is stored in the file `data.txt` next to the word millionth.

## Solution

As with [Level 6](#level-6), `grep` and `|` are needed.

```
bandit7@bandit:~$ ls
data.txt
bandit7@bandit:~$ cat data.txt | grep millionth
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

---

# Level 9

## Task

The password for the next level is stored in the file `data.txt` and is the only line of text that occurs only once.

## Solution

```
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ cat data.txt
L3ZCH71RRxt8Kmy3X3R0NqQTmebcmkQ4
NknAyxnPgpoEcWHizP4TA8ALeIyco1VT
Fmt5ODm3V6Qf1oTF3qEJNWlVcHFdpbuz
nOu0uI9qll3ws9FtaQt7mE4ngAmMAsfE
ksiDcx6JXM6yZSfpf1TlIIlABpb97SAy
NknAyxnPgpoEcWHizP4TA8ALeIyco1VT
[...]
```

The lines in the file are unsorted, and `uniq` can only filter adjacent matching lines using the flag `-u`. To find unique values, `sort` is used first and the results piped to `uniq -u`.

```
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

---

# Level 10

## Task

The password for the next level is stored in the file `data.txt` in one of the few human-readable strings, preceded by several '=' characters.

## Solution

`strings` can be used to filter data.txt to show printable (human-readable) strings. The results are then piped to `grep`, set to find sequences with several '=' characters.

```
bandit9@bandit:~$ strings data.txt | grep ===
========== the
========== password{k
=========== is
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

---

# Level 11

## Task

The password for the next level is stored in the file `data.txt`, which contains base64 encoded data.

## Solution

The data is base64 encoded. This can be decoded using `base64` with the `-d` flag.

```
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
bandit10@bandit:~$ base64 -d data.txt
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

---

# Level 12

## Task

The password for the next level is stored in the file `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

## Solution

`tr <string1> <string2>` is used to substitute characters in input strings, replacing each character in `string1` with the corresponding character in `string2`.

```
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
bandit11@bandit:~$ cat data.txt | tr A-Za-z N-ZA-Mn-za-m
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

---

# Level 13

## Task

The password for the next level is stored in the file `data.txt`, which is a hexdump of a file that has been repeatedly compressed.

For this level it may be useful to create a directory under `/tmp` in which you can work.

Use `mkdir` with a hard to guess directory name. Or better, use the command `mktemp -d`. Then copy the datafile using `cp`, and rename it using `mv` (read the manpages!).

## Solution

An overview of the file/directory manipulation commands used in this level:

- `mkdir <path>` makes a new directory leading to <path>
- `mktemp` creates a temporary file. Adding the `-d` flag causes it to create a temporary directory instead
- `cp <source> <destination>` copies files and directories from the source to destination.
- `mv <source> <destination>` moves files and directories from the source to desitination directory. Also used to rename files and folders.

Also the hexdump and compression/decompression commands used:

- `xxd <file>` creates a hexdump of the file. Adding the `-r` flag reverts the hexdump back into binary.
- `gzip` and `bzip2` creates compressed files using different algorithms. Adding the `-d` flag decompresses the files instead. The files typically have the `.gz` and `.bz2` extensions.
- `tar` creates an archive file from one or multiple files with `-cf`, and extracts files from an archive with `-xf`. The files typically have the `.tar` extension.

We'll first get a temporary working directory where we have write permission using `mkdir` or `mktemp -d`. I used `mktemp -d` for simplicity.

We then copy the data to the temporary directory using `cp` and `cd` over to the temporary directory, then reverse the hexdump of the data using `xxd -r`.

```
bandit12@bandit:~$ mktemp -d
/tmp/tmp.NdBNKOapJs
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ cp data.txt /tmp/tmp.NdBNKOapJs/data_original
bandit12@bandit:~$ cd /tmp/tmp.NdBNKOapJs
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd -r data_original > data
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data  data_original
```

We'll need to find out which decompression algorithm to use, which can be determined from the [file signature](https://en.wikipedia.org/wiki/List_of_file_signatures "Wikipedia article that lists the hex signatures of various file types") of the hexdumps.

To stop cluttering the terminal with the long stdout from reading the files, we can pipe it into `head` to return only the first 10 lines, or `head` could be used in place of `cat`.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ head data_original
00000000: 1f8b 0808 41d4 f767 0203 6461 7461 322e  ....A..g..data2.
00000010: 6269 6e00 0149 02b6 fd42 5a68 3931 4159  bin..I...BZh91AY
[...]
```

Here we see that the file signature is `1f 8b`, corresponding to gzip. We'll have to first add the `.gz` extension to the `data` file using `mv`.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data.gz  data_original
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data  data_original
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data | head
00000000: 425a 6839 3141 5926 5359 a8ff a78f 0000  BZh91AY&SY......
00000010: 1d7f ffdb eb7f fabb 7fa5 efbb 7ef5 fbfd  ............~...
[...]
```

The signature for the decompressed file is `42 5a 68`, corresponding to bzip2. The `.bz2` extension has to be added.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data data.bz2
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ bzip2 -d data.bz2
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data  data_original
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data | head
00000000: 1f8b 0808 41d4 f767 0203 6461 7461 342e  ....A..g..data4.
00000010: 6269 6e00 edd1 4b48 9461 1480 e14f c469  bin...KH.a...O.i
[...]
```

Decompress it again using `gzip`.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data | head
00000000: 6461 7461 352e 6269 6e00 0000 0000 0000  data5.bin.......
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
[...]
```

Here we see a string `data5.bin` in the hexdump, indicating it is an archive. The `.tar` extension has to be added, then the file(s) extracted from the archive with `tar -xf`.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data5.bin  data_original  data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data5.bin | head
00000000: 6461 7461 362e 6269 6e00 0000 0000 0000  data6.bin.......
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
[...]
```

`data5.bin` appears to be another archive.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data5.bin data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data6.bin  data_original  data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data6.bin | head
00000000: 425a 6839 3141 5926 5359 9e50 1aa4 0000  BZh91AY&SY.P....
00000010: 8cff cfdc 6a00 40c0 7df7 e120 5b23 8074  ....j.@.}.. [#.t
[...]
```

`42 5a 68` again, indicating a bzip2 file. The identifying and decompressing/extracting continues until we see something readable, at which point we finally read the file using `cat`.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data6.bin data.bz2
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ bzip2 -d data.bz2
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data | head
00000000: 6461 7461 382e 6269 6e00 0000 0000 0000  data8.bin.......
00000010: 0000 0000 0000 0000 0000 0000 0000 0000  ................
[...]
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ tar -xf data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ ls
data8.bin  data_original  data.tar
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data8.bin | head
00000000: 1f8b 0808 41d4 f767 0203 6461 7461 392e  ....A..g..data9.
00000010: 6269 6e00 0bc9 4855 2848 2c2e 2ecf 2f4a  bin...HU(H,.../J
[...]
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ mv data8.bin data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ gzip -d data.gz
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ xxd data
00000000: 5468 6520 7061 7373 776f 7264 2069 7320  The password is
00000010: 464f 3564 7746 7363 3063 6261 4969 4830  FO5dwFsc0cbaIiH0
00000020: 6838 4a32 6555 6b73 3276 6454 4477 416e  h8J2eUks2vdTDwAn
00000030: 0a                                       .
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ cat data
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

## Extras

Remember to clean up the files in the temporary folder after we're done with them.

```
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ rm -rf /tmp/tmp.NdBNKOapJs
bandit12@bandit:/tmp/tmp.NdBNKOapJs$ cd /tmp/tmp.NdBNKOapJs
-bash: cd: /tmp/tmp.NdBNKOapJs: No such file or directory
```

---

# Level 14

## Task

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`.

For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: `localhost` is a hostname that refers to the machine you are working on.

## Solution

`ssh`, using the `-i` flag, can also be used to log in to a remote machine using a specified private key. This method, [public key cryptography] (https://en.wikipedia.org/wiki/Public-key_cryptography "Wikipedia article on public key cryptography"), is an alternative to using a password to log in.

```
bandit13@bandit:~$ ssh -i sshkey.private bandit14@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit13/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit13/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames
[...]
```

From there, we can access the password for `bandit14` at `/etc/bandit_pass/bandit14`

```
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

---

# Level 15

## Task

The password for the next level can be retrieved by submitting the password of the current level to port `30000` on `localhost`.

## Solution

Netcat, `nc <host> <port>`, is used for TCP and UDP connections and listens. It can also be used to scan for open ports.

```
bandit14@bandit:~$ nc localhost 30000
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

---

# Level 16

## Task

The password for the next level can be retrieved by submitting the password of the current level to port `30001` on `localhost` using SSL/TLS encryption.

## Solution

Attempting to connect to port `30001` using netcat does not give any response. This is because netcat does not provide encryption.

```
bandit15@bandit:~$ nc localhost 30001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
bandit15@bandit:~$
```

We will have to use `openssl s_client` for this. The documentation is [here](https://docs.openssl.org/master/man1/openssl-s_client/ "OpenSSL documentation for openssl s_client").

```
bandit15@bandit:~$ openssl s_client -connect localhost:30001
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
[...]
---
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

closed
bandit15@bandit:~$
```

---

# Level 17

## Task

The credentials for the next level can be retrieved by submitting the password of the current level to a port on `localhost` in the range `31000` to `32000`.

First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t.

There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

Helpful note: Getting "DONE", "RENEGOTIATING" or "KEYUPDATE"? Read the "CONNECTED COMMANDS" section in the manpage.

## Solution

Netcat with the `-z` flag can be used for port scanning to determine which ports are open, but it cannot determine which ports use SSL/TLS.

It would be possible to work your way through each found port using `nc` to filter out the connections not using SSL/TLS, then attempt connection to the remaining ports using OpenSSL.

```
bandit16@bandit:~$ nc -z localhost 31000-32000
Connection to localhost (127.0.0.1) 31046 port [tcp/*] succeeded!
Connection to localhost (127.0.0.1) 31518 port [tcp/*] succeeded!
Connection to localhost (127.0.0.1) 31691 port [tcp/*] succeeded!
Connection to localhost (127.0.0.1) 31790 port [tcp/*] succeeded!
Connection to localhost (127.0.0.1) 31960 port [tcp/*] succeeded!
bandit16@bandit:~$ nc localhost 31046
test
test
^C
bandit16@bandit:~$ nc localhost 31518
test
^C
bandit16@bandit:~$ nc localhost 31691
test
test
^C
bandit16@bandit:~$
[...]
```

However, `nmap` with its service/version detection functionality using the `-sV` flag lets us scan and find out which ports use SSL, including the service each port is running.

```
bandit16@bandit:~$ nmap -sV -p31000-32000 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-21 15:06 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00021s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.94SVN%T=SSL%I=7%D=7/21%Time=687E578C%P=x86_64-pc-linu
SF:x-gnu%r(GenericLines,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x2
SF:0current\x20password\.\n")%r(GetRequest,32,"Wrong!\x20Please\x20enter\x
SF:20the\x20correct\x20current\x20password\.\n")%r(HTTPOptions,32,"Wrong!\
SF:x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n")%r(RTS
SF:PRequest,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20
SF:password\.\n")%r(Help,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x
SF:20current\x20password\.\n")%r(FourOhFourRequest,32,"Wrong!\x20Please\x2
SF:0enter\x20the\x20correct\x20current\x20password\.\n")%r(LPDString,32,"W
SF:rong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\.\n")
SF:%r(SIPOptions,32,"Wrong!\x20Please\x20enter\x20the\x20correct\x20curren
SF:t\x20password\.\n");

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 156.08 seconds
```

Only ports `31518` and `31790` use SSL, with `31518` hosting an echo service. This makes `31790` a likely candidate for connecting to.

```
openssl s_client -connect localhost:31790
CONNECTED(00000003)
Can't use SSL_get_servername
[...]
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
KEYUPDATE
```

We get `KEYUPDATE`, which the note in the original task tells us to look at the "[Connected Commands](https://docs.openssl.org/3.0/man1/openssl-s_client/#connected-commands "Connected commands section in the OpenSSL documentation for openssl s_client")" section in the OpenSSL manual.

The page states that "When used interactively (which means neither `-quiet` nor `-ign_eof` have been given), then certain commands are also recognized which perform special operations. These commands are a letter which must appear at the start of a line. They are listed below."

Lowercase k appears at the start of the password for this level, triggering a key update. This is avoided by including `-ign_eof` in the openssl s_client command.

```
bandit16@bandit:~$ openssl s_client -connect localhost:31790 -ign_eof
CONNECTED(00000003)
Can't use SSL_get_servername
[...]
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
Correct!
-----BEGIN RSA PRIVATE KEY-----
[...]
```

An RSA private key is given, which can be used to connect to the next level as was done previously on [Level 14](#level-14).

The RSA key is written to a file in a temporary directory, then used to connect to the next level.

```
bandit16@bandit:~$ mktemp -d
/tmp/tmp.I63MVMadit
bandit16@bandit:~$ cd /tmp/tmp.I63MVMadit
bandit16@bandit:/tmp/tmp.I63MVMadit$ echo "-----BEGIN RSA PRIVATE KEY-----
[...]
-----END RSA PRIVATE KEY-----" > key.private
bandit16@bandit:/tmp/tmp.I63MVMadit$ ls
key.private
bandit16@bandit:/tmp/tmp.I63MVMadit$ ssh -i key.private bandit17@localhost -p 2220
[...]
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0664 for 'key.private' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "key.private": bad permissions
bandit17@localhost: Permission denied (publickey).
```

We receive a warning that the permissions for the private key file are too open. The file permissions can be inspected using `ls -l`

```
bandit16@bandit:/tmp/tmp.I63MVMadit$ ls -l
total 4
-rw-rw-r-- 1 bandit16 bandit16 1675 Jul 21 15:37 key.private
```

The first column contains ten characters, the first of which indicates the file type.

The next group of three characters indicate read (`r`), write (`w`), and execute (`x`) permissions for the owner.

The subsequent three characters indicate `r` `w` `x` permissions for the group, and the last three characters indicate `r` `w` `x` permissions for everyone else.

For example, `rwx------` indicates read, write, execute permissions for only the owner, while `rwxrwxrwx` indicates read, write, execute permissions for the owner, group, and everyone else.

These can be changed using `chmod <option> <file or directory>`. I prefer to change permissions using numeric mode, so in this case setting it to `rwx------` would look like `chmod 700 key.private`, which is equivalent to ``chmod u=rwx,g=,o= key.private`.

```
bandit16@bandit:/tmp/tmp.I63MVMadit$ chmod 700 key.private
bandit16@bandit:/tmp/tmp.I63MVMadit$ ls -l
total 4
-rwx------ 1 bandit16 bandit16 1675 Jul 21 15:37 key.private
bandit16@bandit:/tmp/tmp.I63MVMadit$ ssh -i key.private bandit17@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
[...]
  Enjoy your stay!

bandit17@bandit:~$
```

Once logged in to bandit17, we can get the password from `/etc/bandit_pass/bandit17`

```
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
```

---

# Level 18

## Task

There are 2 files in the homedirectory: `passwords.old` and `passwords.new`. The password for the next level is in `passwords.new` and is the only line that has been changed between `passwords.old` and `passwords.new`.

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19.

## Solution

`diff <file1> <file2>` is used to compare the two files in the home directory, with the password taken from `passwords.new`.

```
bandit17@bandit:~$ ls
passwords.new  passwords.old
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> C6XNBdYOkgt5ARXESMKWWOUwBeaIQZ0Y
```

---

# Level 19

## Task

The password for the next level is stored in a file `readme` in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

## Solution

Attempting to ssh in to bandit18 immediately kicks me out.

```
jsjs2401>ssh bandit18@bandit.labs.overthewire.org -p 2220
[...]
                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:

      ,----..            ,----,          .---.
     /   /   \         ,/   .`|         /. ./|
    /   .     :      ,`   .'  :     .--'.  ' ;
   .   /   ;.  \   ;    ;     /    /__./ \ : |
  .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
  ;   |  ; \ ; | |    :     | /___/ \ |    ' '
  |   :  | ; | ' ;    |.';  ; ;   \  \;      :
  .   |  ' ' ' : `----'  |  |  \   ;  `      |
  '   ;  \; /  |     '   :  ;   .   \    .\  ;
   \   \  ',  /      |   |  '    \   \   ' \ |
    ;   :    /       '   :  |     :   '  |--"
     \   \ .'        ;   |.'       \   \ ;
  www. `---` ver     '---' he       '---" ire.org


Welcome to OverTheWire!
[...]
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

Reading around on the `ssh` manual page, I noticed that `ssh` could also be used to send commands to be ran as the user, outside of being used in interactive sessions.

Knowing that the password for the next level is stored in `readme` in the home directory, we can add `cat readme` to the ssh command.

```
jsjs2401>ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

## Extras

Poking around in the `.bashrc` file on bandit18's home directory, we see that the only difference is the addition of the final two lines:

```
jsjs2401>ssh bandit18@bandit.labs.overthewire.org -p 2220 cat .bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

[...]

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi
echo 'Byebye !'
exit 0
```

However, the snippet of code where it returns from `.bashrc` if not running interactively is of interest:

```
# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac
```

Checking the set shell flags (`$-`) with `ssh` into bandit17, we see that `i` is indeed included.

```
bandit17@bandit:~$ echo $-
himBHs
```

Looking up "interactive session" on the `ssh` manual page, I also see that:

> If an interactive session is requested, ssh by default will only request a pseudo-terminal (pty) for interactive sessions when the client has one. The flags -T and -t can be used to override this behaviour.

Adding `-T` when using `ssh` disables pseudo-terminal allocation, which could prevent `i` from appearing in the set shell flags.

Trying this, we indeed see that we can bypass the `exit 0` command in `.bashrc`, and are able to get the password for the next level.

```
jsjs2401>ssh -T bandit18@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
echo $-
hBs
ls
readme
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

Alternatively, using `ssh` as originally intended (to run a command) can also be used to start a separate bash shell to similar effect as using `-T`.

```
jsjs2401>ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash
[...]
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

---

# Level 20

## Task

To gain access to the next level, you should use the setuid binary in the homedirectory.

Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

## Solution

I followed the instructions and found that the binary sets the user ID to the file owner (bandit20) and executes a command.

It was then a simple matter to get the password from `/etc/bandit_pass/bandit20`.

```
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit20 bandit19 14884 Apr 10 14:23 bandit20-do
bandit19@bandit:~$ file bandit20-do
./bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=35d353cf6d732f515a73f50ed205265fe1e68f90, for GNU/Linux 3.2.0, not stripped
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

---

# Level 21

## Task

There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument.

It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

NOTE: Try connecting to your own network daemon to see if it works as you think.

## Solution

For this level, the given binary `suconnect` connects to a user-defined port to receive the password for bandit20.

```
bandit20@bandit:~$ ls
suconnect
bandit20@bandit:~$ ./suconnect
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
```

This means that we'll need to set up a background process to wait for a connection at that port, and then send the password once a connection is made to it.

The `-l` flag for `nc` is used, which tells `nc` to listen for an incoming connection instead of initiating one to a remote host. With `nc -l -p 2401`, we've set up a network process that listens at port 2401.

To send a message through this, we use `|` to pipe the password through the process, `echo <password> | nc -l -p 2401`.

```
bandit20@bandit:~$ echo <password> | nc -l -p 2401
-bash: syntax error near unexpected token `|'
bandit20@bandit:~$ echo 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401
^C
bandit20@bandit:~$
```

However, this does let us return to the command prompt until we exit the process because `nc` is running. We'll need to make `nc` run in the background using the `&` symbol.

We'll do this and test if it works by connecting to our own port.

```
bandit20@bandit:~$ echo 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401 &
[1] 1573294
bandit20@bandit:~$ nc localhost 2401
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
test
test
^C
bandit20@bandit:~$ nc localhost 2401
[1]+  Done                    echo 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401
bandit20@bandit:~$ nc localhost 2401
bandit20@bandit:~$
```

We can see that after the network daemon is done sending the password, it remains open and simply echoes back whatever is sent to it until it is disconnected from.

After disconnection, the process ends and reports that it is completed. Subsequent attempts to connect to the port show that there is no longer any process listening on it. 

With this, we can get the password for the next level.

```
bandit20@bandit:~$ echo 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401 &
[1] 1582268
bandit20@bandit:~$ ./suconnect 2401
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

## Extras

A more naieve approach to running the the network process in the background would be to use two terminals, one to run the network process and another to start `./suconnect` afterwards.

A slightly cleaner way to write the `nc` command would be to add the `-n` flag to `echo`, suppressing the trailing newline character and making the process exit once it sends the bandit20 password.

```
bandit20@bandit:~$ echo -n 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401 &
[1] 1594601
bandit20@bandit:~$ ./suconnect 2401
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
[1]+  Done                    echo -n 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO | nc -l -p 2401
```

---

# Level 22

## Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

## Solution

Cron is used to run scripts or commands at user-defined intervals.

Cronjobs are defined in a file (the crontab) following the format, with the `<user>` being optional:

```
<minute> <hour> <day> <month> <weekday> <user> <command>
```

The following example runs `/etc/cron.d/script.sh` as the user `bandit21` at 11.30am on the 1st day of month, every month, every weekday.

```
30 11 1 * * bandit21 /etc/cron.d/script.sh
```

We first check out what is being run in the crontab for the next level (bandit22).

```
bandit21@bandit:~$ ls /etc/cron.d
clean_tmp  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

The first cronjob, on reboot, runs `/usr/bin/cronjob_bandit22.sh` as bandit22 in the background and sends any output to the bin. The second cronjob does the same but on every minute of every hour, instead of only on reboot.

We'll check out what the script `/usr/bin/cronjob_bandit22.sh` does.

```
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

This tells us that every minute, the file `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` has its permissions changed to `rw-r--r--`, and then the password for bandit22 is written to it.

```
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

---

# Level 23

## Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

## Solution

We do the same initial steps as in the level before, and inspect `/usr/bin/cronjob_bandit23.sh`.

```
bandit22@bandit:~$ ls /etc/cron.d
clean_tmp  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

The `$(<command>)` syntax passes the output of the command as an argument in another command.

The `whoami` command outputs the current user. Since the script is running as `bandit23`, the input to the next command should be `bandit23`.

The next command uses two new commands, `md5sum` and `cut`. `md5sum` prints the MD5 checksums of the input file, and `cut` is used here to return only the text before the first space character.

Running the entire command, we see that the `mytarget` variable is `8ca319486bfbbc3663ea0fbe81326349`.

```
bandit22@bandit:~$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

This gives us the path for where the password is stored, `/tmp/8ca319486bfbbc3663ea0fbe81326349`.

```
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

---

# Level 24

## Task

A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

## Solution

The same starting steps as before, and we get a longer script in `/usr/bin/cronjob_bandit24.sh` this time.

```
bandit23@bandit:~$ ls /etc/cron.d
clean_tmp  cronjob_bandit22  cronjob_bandit23  cronjob_bandit24  e2scrub_all  otw-tmp-dir  sysstat
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

The script itself tells us what it's doing, `echo "Executing and deleting all scripts in /var/spool/$myname/foo:"`, but let's break down what exactly is being done.

The first part simply changes the directory based on the current user, in this case to `/var/spool/bandit24/foo`.

```
myname=$(whoami)

cd /var/spool/$myname/foo
```

The next part, `for i in * .*;`, enters a for loop that performs the actions enclosed between `do` and `done` on every file/directory `*` and hidden file/directory `.*` in the directory.

The if-then statement `if [ "$i" != "." -a "$i" != ".." ];` checks that the file/directory being worked on by the for loop isn't the current directory `.` or parent directory `..`.

The next block of code first gets the owner of the file using `stat --format "%U" ./$i`, and if the owner is bandit23 (who we currently are logged in as), it executes the script stored in the file using `timeout`, which will also kill the command if it is still running after 60 seconds.

```
owner="$(stat --format "%U" ./$i)"
if [ "${owner}" = "bandit23" ]; then
    timeout -s 9 60 ./$i
fi
```

The last part in the for loop, `rm -f ./$i` deletes the file regardless of ownership.

To summarise, we know that:

1. Every minute, the cronjob scans through and deletes all files in the `/var/spool/bandit24/foo` directory.
2. If the file is owned by bandit23, the cronjob runs the file as bandit24.

With this, we now know to write a script that will be executed as bandit24 that will give us the password from `/etc/bandit_pass/bandit24.

We start by getting a temporary working directory and starting up `nano` to edit a file `password.sh`.

```
bandit23@bandit:~$ mktemp -d
/tmp/tmp.FGa4yfYnEF
bandit23@bandit:~$ cd /tmp/tmp.FGa4yfYnEF
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ nano password.sh
```

Every script starts with a shebang `#!` to specify the interpreter. We can use `/bin/bash`, just like in the script from `/usr/bin/cronjob_bandit24.sh`.

We then write a command to access bandit24's password and write it to our working directory.

```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.FGa4yfYnEF/password
```

We should also add our own file `password` to the working directory.

```
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ touch password
```

We'll need to edit the permissions to allow others to execute our script file, and for others to access to our working directory and password file, since the user bandit24 will be running the script. Right now our files only have read access for everyone else.

```
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ ls -l
total 4
-rw-rw-r-- 1 bandit23 bandit23  0 Jul 22 09:37 password
-rw-rw-r-- 1 bandit23 bandit23 74 Jul 22 09:24 password.sh
```

We do this using `chmod`, which we used back in [Level 17](#level-17), to add permissions for others.

```
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ chmod 777 /tmp/tmp.FGa4yfYnEF
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ chmod 777 password
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ chmod 777 password.sh
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ ls -l
total 4
-rw-rw-rw- 1 bandit23 bandit23  0 Jul 22 09:37 password
-rwxrwxr-x 1 bandit23 bandit23 74 Jul 22 09:24 password.sh
```

We copy the file into `/var/spool/bandit24/foo/` and wait for a minute.

```
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ cp password.sh /var/spool/bandit24/foo/password.sh
bandit23@bandit:/tmp/tmp.FGa4yfYnEF$ cat password
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

---

# Level 25

## Task

A daemon is listening on port `30002` and will give you the password for `bandit25` if given the password for `bandit24` and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time.

## Solution

We start by connecting to port `30002` using `nc` and seeing what it does.

```
bandit24@bandit:~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000
Wrong! Please enter the correct current password and pincode. Try again.
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0001
Wrong! Please enter the correct current password and pincode. Try again.
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0002
Wrong! Please enter the correct current password and pincode. Try again.
^C
```

We could manually work our way through all 10,000 possiblities, but we could also write a script to do that for us.

We first get a working directory and create a file for our script, `script.sh`.

```
bandit24@bandit:~$ mktemp -d
/tmp/tmp.OI7GpstwZ5
bandit24@bandit:~$ cd /tmp/tmp.OI7GpstwZ5
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ nano script.sh
```

Then we write our script, appending the possible passwords to `passwords.txt`. To get a range of numbers for the `for` loop, the syntax is `{0..9999}`, but that only produces `1, 2, 3, ...` instead of the 4-digit numbers we want. For that, the syntax is `{0000..9999}`.

```bash
#!/bin/bash

password="gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"

for i in {0000..9999}
do
    echo $password $i >> passwords.txt
done
```

We run the script and are denied as it didn't have execute permissions. This is easily changed with `chmod`.

```
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ ./script.sh
-bash: ./script.sh: Permission denied
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ ls -l
total 4
-rw-rw-r-- 1 bandit24 bandit24 131 Jul 22 10:11 script.sh
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ chmod 700 script.sh
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ ./script.sh
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ ls
passwords.txt  script.sh
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ head passwords.txt
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0000
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0001
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 0002
[...]
```

Now we send this to the server by piping the text in the file to `nc`, then saving the result into `results.txt`.

```
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ cat passwords.txt | nc localhost 30002 > results.txt
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ ls
passwords.txt  results.txt  script.sh
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ head results.txt
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
[...]
```

To find the line with the correct password, we use `grep` with the `-v` flag to invert matches with "Wrong".

```
bandit24@bandit:/tmp/tmp.OI7GpstwZ5$ cat results.txt | grep -v Wrong
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

---

# Level 26

## Task

Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

NOTE: if you’re a Windows user and typically use Powershell to ssh into bandit: Powershell is known to cause issues with the intended solution to this level. You should use command prompt instead.

## Solution

An SSH key for bandit26 is available in the home directory when logging in to bandit25. However, trying to use it to log in to bandit 26 immediately logs us out.

```
bandit25@bandit:~$ ls
bandit26.sshkey
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
[...]
 Enjoy your stay!

  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
Connection to localhost closed.
bandit25@bandit:~$
```

As mentioned in the task, we'll need to find out what shell bandit26 is using. This can be found in the `/etc/passwd` file, which lists the default shells of the users.

There are a lot of entries so we'll `grep bandit26` to find the relevant line, then open the referenced file.

```
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

The script uses `more` to open the file `text.txt` in bandit26's home directory, then exits.

There are only a few things to look at here: `more`, and `text.txt` in bandit26's home directory.

```
bandit25@bandit:~$ cat /home/bandit26/text.txt
cat: /home/bandit26/text.txt: Permission denied
bandit25@bandit:~$ ls -l /home/bandit26/
total 20
-rwsr-x--- 1 bandit27 bandit26 14884 Apr 10 14:23 bandit27-do
-rw-r----- 1 bandit26 bandit26   258 Apr 10 14:23 text.txt
```

We are unable to look into `text.txt` since we do not have read permission for the file, so the only option is to look into `more`.

Reading the documentation, `more` lets us page through text one screenful at a time, and also allows for interactive commands based on `vi`.

Unfortunately, the contents in `text.txt` are too short and can all be displayed in one screen. `more` only goes into interactive mode when the contents are long enough.

I created three files, `short.txt` with just one line of text, `medium.txt` with enough text to fill just under one screen, and `long.txt` with multiple lines of text to demonstrate this.

```
bandit25@bandit:~$ mktemp -d
/tmp/tmp.5j5BGtuKhf
bandit25@bandit:~$ cd /tmp/tmp.5j5BGtuKhf
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ nano short.txt
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ nano medium.txt
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ nano long.txt
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ ls
long.txt  medium.txt  short.txt
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ more short.txt
hello :)
```

Entering the command `more long.txt` shows this:

![Screenshot of the terminal when more command is used on long.txt](https://github.com/jsjs2401/OverTheWire/blob/main/01_Bandit/bandit25-more-long-txt.png)

If we enter `more medium.txt`, we see that it did not enter interactive mode.

![Screenshot of the terminal when more command is used on medium.txt](https://github.com/jsjs2401/OverTheWire/blob/main/01_Bandit/bandit25-more-medium-txt-1.png)

However, when we resize the window to be unable to fit all of `medium.txt`, we are able to enter interactive mode with `more`.

![Screenshot of the resized terminal when more command is used on medium.txt](https://github.com/jsjs2401/OverTheWire/blob/main/01_Bandit/bandit25-more-medium-txt-2.png)

With this in mind, we can try resizing the terminal and logging in to bandit26 again.

```
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ rm -rf /tmp/tmp.5j5BGtuKhf
bandit25@bandit:/tmp/tmp.5j5BGtuKhf$ cd ~
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
[...]
```

![Screenshot of the resized terminal after successful SSH into bandit26](https://github.com/jsjs2401/OverTheWire/blob/main/01_Bandit/bandit25-more-ssh.png)

Once we're in `more` interactive mode, we can resize the window and start up an editor by pressing `v`.

We then set the shell to one that is not the user default by entering `:set shell=/bin/bash`, and then enter that shell by entering `:shell`.

We're now logged in to bandit26.

![Screenshot of the terminal after successful SSH into bandit26 and entering bash shell](https://github.com/jsjs2401/OverTheWire/blob/main/01_Bandit/bandit25-more-ssh-bash-shell.png)

It's then a simple matter of getting the password for bandit26.

```
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ
```

We use `exit` to exit the shell, then enter `:q` to exit vim, returning to `more` interactive mode. Press `q` to exit interactive mode and be logged out from bandit26.

---

# Level 27

## Task

Good job getting a shell! Now hurry and grab the password for bandit27!

## Solution

Like back in [Level 20](#level-20), we get a binary to run commands as another user—in this case bandit27.

```
bandit26@bandit:~$ ls -l
total 20
-rwsr-x--- 1 bandit27 bandit26 14884 Apr 10 14:23 bandit27-do
-rw-r----- 1 bandit26 bandit26   258 Apr 10 14:23 text.txt
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```

---

# Level 28

## Task

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

## Solution

We create a temporary directory to clone the respository and find the password there.3

`git clone` is used, with the [syntax](https://git-scm.com/docs/git-clone#_git_urls "Syntax of URLs for git clone") being `ssh://<user>@<host>:<port>/<path-to-git-repo>`

```
bandit27@bandit:~$ mktemp -d
/tmp/tmp.kkOUXuV8TC
bandit27@bandit:~$ cd /tmp/tmp.kkOUXuV8TC
bandit27@bandit:/tmp/tmp.kkOUXuV8TC$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit27/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit27-git@localhost's password:
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```

From there, we look into the repository to find the password.

```
bandit27@bandit:/tmp/tmp.kkOUXuV8TC$ ls -A
repo
bandit27@bandit:/tmp/tmp.kkOUXuV8TC$ cd repo
bandit27@bandit:/tmp/tmp.kkOUXuV8TC/repo$ ls -A
.git  README
bandit27@bandit:/tmp/tmp.kkOUXuV8TC/repo$ cat README
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

---

# Level 29

## Task

There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo via the port 2220. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

## Solution

We do the same initial steps as before, cloning the repository and looking into its files.

```
bandit28@bandit:~$ mktemp -d
/tmp/tmp.erbTy5FUT1
bandit28@bandit:~$ cd /tmp/tmp.erbTy5FUT1
bandit28@bandit:/tmp/tmp.erbTy5FUT1$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
Cloning into 'repo'...
[...]
bandit28@bandit:/tmp/tmp.erbTy5FUT1$ ls -A
repo
bandit28@bandit:/tmp/tmp.erbTy5FUT1$ cd repo
bandit28@bandit:/tmp/tmp.erbTy5FUT1/repo$ ls -A
.git  README.md
bandit28@bandit:/tmp/tmp.erbTy5FUT1/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

The password has been removed out, so we should look elsewhere. We can check the history of the repository using `git log`, showing the commit log.

```
bandit28@bandit:/tmp/tmp.erbTy5FUT1/repo$ git log
commit 674690a00a0056ab96048f7317b9ec20c057c06b (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    fix info leak

commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

commit a5fdc97aae2c6f0e6c1e722877a100f24bcaaa46
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    initial commit of README.md
```

`fix info leak` probably means that it was the latest commit that removed the password, so we should check that commit using `git show`.

`git show <object>` is used to show all changes made in that particular commit.

```
bandit28@bandit:/tmp/tmp.erbTy5FUT1/repo$ git show fb0df1358b1ff146f581651a84bae622353a71c0
commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: <TBD>
+- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

## Extras

Alternatively, we could also use the syntax for `git show` shown in the [git documentation](https://git-scm.com/docs/git-show#Documentation/git-show.txt-gitshownext10DocumentationREADME "Git documentation, examples for git show").

> **git show next~10:Documentation/README**
> 
> - Shows the contents of the file Documentation/README as they were current in the 10th last commit of the branch next.

```
bandit28@bandit:/tmp/tmp.erbTy5FUT1/repo$ git show master~1:README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```

---

# Level 30

## Task

There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo via the port 2220. The password for the user bandit29-git is the same as for the user bandit29.

Clone the repository and find the password for the next level.

## Solution

Same as before, we clone the repository into our working directory and inspect the README.md file, also checking through `git log`.

```
bandit29@bandit:~$ mktemp -d
/tmp/tmp.7qKu8fhTxW
bandit29@bandit:~$ cd /tmp/tmp.7qKu8fhTxW
bandit29@bandit:/tmp/tmp.7qKu8fhTxW$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
[...]
bandit29@bandit:/tmp/tmp.7qKu8fhTxW$ cd repo
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ ls -A
.git  README.md
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git log
commit 3b8b91fc3c48f1a19d05670fd45d3e3f2621fcfa (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    fix username

commit 8d2ffeb5e45f87d0abb028aa796e3ebb63c5579c
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    initial commit of README.md
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git show master~1:README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit29
- password: <no passwords in production!>
```

`<no passwords in production!>` implies that the passwords are in a separate development branch.

We can use `git branch` to see existing branches. `-a` is included to see remote branches, otherwise we would only see local branches.

```
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git branch --list
* master
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git branch --list -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

`git switch` or `git checkout` is used to switch branches.

Switching to a remote branch needs the `--detach` option. This creates a local clone of the branch.

```
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git switch remotes/origin/dev
fatal: a branch is expected, got remote branch 'origin/dev'
hint: If you want to detach HEAD at the commit, try again with the --detach option.
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git switch --detach remotes/origin/dev
HEAD is now at a97d0db add data needed for development
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ git branch -a
* (HEAD detached at origin/dev)
  master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

Here we find the password for the next level.

```
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ ls -A
code  .git  README.md
bandit29@bandit:/tmp/tmp.7qKu8fhTxW/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

---

# Level 31

## Task

There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo via the port 2220. The password for the user bandit30-git is the same as for the user bandit30.

Clone the repository and find the password for the next level.

## Solution

We go through the same steps as in the previous few levels,

```
bandit30@bandit:~$ mktemp -d
/tmp/tmp.faBGObl8FM
bandit30@bandit:~$ cd /tmp/tmp.faBGObl8FM
bandit30@bandit:/tmp/tmp.faBGObl8FM$ git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
Cloning into 'repo'...
[...]
bandit30@bandit:/tmp/tmp.faBGObl8FM$ cd repo
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ ls -A
.git  README.md
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ cat README.md
just an epmty file... muahaha
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ git log
commit fb05775f973256dc6d8d5bb6a8e6b96b0d8795c8 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:24 2025 +0000

    initial commit of README.md
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

There are no other details in previous commits or branches.

We can also check the tags using `git tag`. This is used with `-l` to list tags.

Once we know the tags, we can use `git show` to show their message.

```
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ git tag -l
secret
bandit30@bandit:/tmp/tmp.faBGObl8FM/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

---

# Level 32

## Task

There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo via the port 2220. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

## Solution

```
bandit31@bandit:~$ mktemp -d
/tmp/tmp.4hov8ru73o
bandit31@bandit:~$ cd /tmp/tmp.4hov8ru73o
bandit31@bandit:/tmp/tmp.4hov8ru73o$ git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
Cloning into 'repo'...
[...]
bandit31@bandit:/tmp/tmp.4hov8ru73o$ cd repo
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ ls -A
.git  .gitignore  README.md
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

Out task is to push a file to a remote repository. To do this, we need a few commands: `git add`, `git commmit`, and `git push`. Additionally, the `.gitignore` file instructs `git add` to ignore files ending with `.txt`.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ cat .gitignore
*.txt
```

We first create and write the message to a new file `key.txt`.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ nano key.txt
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ cat key.txt
May I come in?
```

Attempting to run `git add .` by itself results in no files added. We can see what files have been staged using `git diff --staged --name-only`.

It also ignores the command when `key.txt` is specified.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git add .
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git diff --staged --name-only
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git add key.txt
The following paths are ignored by one of your .gitignore files:
key.txt
hint: Use -f if you really want to add them.
hint: Turn this message off by running
hint: "git config advice.addIgnoredFile false"
```

We could follow the suggestion to use the `-f` tag, or we could remove `.txt` from `.gitignore`. Both options are shown separately below, with the staged area reset between them using `git reset`.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git add -f key.txt
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git diff --staged --name-only
key.txt
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git reset
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ rm .gitignore
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git add key.txt
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git diff --staged --name-only
key.txt
```

The repository was restored to include `.gitignore` again, and we'll proceed with using `git add -f key.txt`.

Note that restoring the repository did not remove key.txt, as it is still untracked, even though it is still in the staged area.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git restore .
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ ls -A
.git  .gitignore  key.txt  README.md
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git diff --staged --name-only
key.txt
```

We're now ready to commit the changes using `git commit -a`. The `-a` flag automatically stages files that have been modified and deleted, but not new (untracked) files.

It opens up nano to enter a message which could be anything, but I typed `Add key.txt`.

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git commit -a
Unable to create directory /home/bandit31/.local/share/nano/: No such file or directory
It is required for saving/loading search history or cursor positions.

[master 4e71748] Add key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git log
commit 4e717489ce4e3280b3fb902e08f5d39bd7a0ef86 (HEAD -> master)
Author: bandit31 <bandit31@overthewire.org>
Date:   Tue Jul 22 16:23:11 2025 +0000

    Add key.txt

commit 9f2814daa679b29d2c8f78f2766e7e9332445a41 (origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:26 2025 +0000

    initial commit
```

We'll then push the new commit to the remote repository using `git push origin master`, as shown in the [example section of the git-push doucmentation](https://git-scm.com/docs/git-push#_examples "Example section of the Git documentation for git push").

```
bandit31@bandit:/tmp/tmp.4hov8ru73o/repo$ git push origin master
[...]
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
```

---

# Level 33

## Task

After all this git stuff, it’s time for another escape. Good luck!

## Solution

We are greeted by aggressive text upon logging in.

```
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: Permission denied
>> printenv
sh: 1: PRINTENV: Permission denied
>> 123
sh: 1: 123: Permission denied
>> $
sh: 1: $: Permission denied
```

As the introductory message suggests, all alphabetical input is converted to uppercase, but numbers and symbols are not. This is then sent to be executed, presumably in the `sh` shell. We need to find a way to break out of the `UPPERCASE SHELL` using only symbols and numbers.

`$0` in bash is used to refer to the name of the shell script, or if bash is directly invoked, then `$0` refers to bash (or sh, or /bin/bash, or /bin/sh) itself and can be used to invoke bash again.

```
>> $0
$ echo $0
sh
```

Once we're in a regular shell, we can examine the home folder contents. We see that `uppershell` is owned by bandit33.

```
$ ls -A
.bash_logout  .bashrc  .profile  uppershell
$ ls -lA
total 28
-rw-r--r-- 1 root     root       220 Mar 31  2024 .bash_logout
-rw-r--r-- 1 root     root      3771 Mar 31  2024 .bashrc
-rw-r--r-- 1 root     root       807 Mar 31  2024 .profile
-rwsr-x--- 1 bandit33 bandit32 15140 Apr 10 14:23 uppershell
```

The `s` attribute in the permissions indicates that the file will run with the user ID of the owner when executed, in this case bandit33.

We can confirm this using `whoami`.

```
$ whoami
bandit33
```

With this, we can get the password from the `/etc/bandit_pass` folder.

```
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

---

# Level 34

```
bandit33@bandit:~$ ls -A
.bash_logout  .bashrc  .profile  README.txt
bandit33@bandit:~$ cat README.txt
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
