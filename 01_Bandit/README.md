# Introduction

[Bandit](https://overthewire.org/wargames/bandit/ "Landing page for Bandit wargame") serves as an introduction to basic Linux, and to Git in the later levels.

Windows' command prompt was used for logging in to each level.

# Overview

| Level    | Concepts Needed                                        | New Commands Used                            |
| :------: | :----------------------------------------------------- | :------------------------------------------- |
| Level 0  | SSH login                                              | ssh                                          |
| Level 1  | Reading files                                          | ls, cat                                      |
| Level 2  | Accessing files with special characters                |                                              |
| Level 3  | Accessing files with spaces in filename                |                                              |
| Level 4  | Changing directories, accessing hidden files           | cd                                           |
| Level 5  | File types                                             | file                                         |
| Level 6  | File sizes, non-executable files, grep and piping      | du, find, grep, \|                           |
| Level 7  | File user and file group ownership                     |                                              |
| Level 8  | Grep and piping of file contents                       |                                              |
| Level 9  | Sorting and finding unique values                      | sort, uniq                                   |
| Level 10 | Finding specific strings in a file                     | strings                                      |
| Level 11 | Base64 encoding and decoding                           | base64                                       |
| Level 12 | Rot13 substitution cipher                              | tr                                           |
| Level 13 | Hexdump, compression/decompression and file signatures | mkdir, mktemp, cp, mv, xxd, gzip, bzip2, tar |
| Level 14 | SSH login with private key                             |                                              |
| Level 15 | Netcat and network communication                       | nc                                           |
| Level 16 | SSL encryption                                         | openssl s_client                             |
| Level 17 | Port scanning, service detection, file permissions     | nmap, chmod                                  |
| Level 18 | Finding differences between files                      | diff                                         |
| Level 19 | Using SSH outside of interactive sessions              |                                              |
| Level 20 | Set user ID (setuid), file permissions                 |                                              |

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
