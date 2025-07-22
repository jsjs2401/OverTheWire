# OverTheWire
[OverTheWire](https://overthewire.org) is a community that offers curated wargames to "to learn and practice security concepts in the form of fun-filled games".

This repository is mostly to serve as a quick reference for the things I've learned playing through the wargames.

The games are played in the order suggested on the website—starting with [Bandit](https://overthewire.org/wargames/bandit/)—and each documented in its own directory.

# Bandit Overview

| Level    | Concepts Needed                                             | New Commands Used                                               |
| :------: | :---------------------------------------------------------- | :-------------------------------------------------------------- |
| Level 0  | SSH login                                                   | ssh                                                             |
| Level 1  | Reading files                                               | ls, cat                                                         |
| Level 2  | Accessing files with special characters                     |                                                                 |
| Level 3  | Accessing files with spaces in filename                     |                                                                 |
| Level 4  | Changing directories, accessing hidden files                | cd                                                              |
| Level 5  | File types                                                  | file                                                            |
| Level 6  | File sizes, non-executable files, grep and piping           | du, find, grep, \|                                              |
| Level 7  | File user and file group ownership                          |                                                                 |
| Level 8  | Grep and piping of file contents                            |                                                                 |
| Level 9  | Sorting and finding unique values                           | sort, uniq                                                      |
| Level 10 | Finding specific strings in a file                          | strings                                                         |
| Level 11 | Base64 encoding and decoding                                | base64                                                          |
| Level 12 | Rot13 substitution cipher                                   | tr                                                              |
| Level 13 | Hexdump, compression/decompression and file signatures      | mkdir, mktemp, cp, mv, xxd, gzip, bzip2, tar, rm                |
| Level 14 | SSH login with private key                                  |                                                                 |
| Level 15 | Netcat and network communication                            | nc                                                              |
| Level 16 | SSL encryption                                              | openssl s_client                                                |
| Level 17 | Port scanning, service detection, file permissions          | nmap, chmod                                                     |
| Level 18 | Finding differences between files                           | diff                                                            |
| Level 19 | SSH remote command execution, shells, bash scripting        |                                                                 |
| Level 20 | Set user ID (setuid), file permissions                      |                                                                 |
| Level 21 | Background processes, network daemons, setuid binary        |                                                                 |
| Level 22 | Cronjobs, bash scripting                                    |                                                                 |
| Level 23 | Cronjobs, bash scripting, bash variables                    | whoami, md5sum, cut                                             |
| Level 24 | Cronjobs, bash scripting, bash variables, file permissions  | stat, timeout, nano                                             |
| Level 25 | Brute forcing passwords, bash scripting, netcat             |                                                                 |
| Level 26 | Shell environments, vim                                     | more, set                                                       |
| Level 27 | Setuid binary                                               |                                                                 |
| Level 28 | Git introduction                                            | git clone                                                       |
| Level 29 | Git commit history                                          | git log, git show                                               |
| Level 30 | Git branching                                               | git branch, git switch, git checkout                            |
| Level 31 | Git tagging                                                 | git tag                                                         |
| Level 32 | Git add, commit, push, .gitignore                           | git add, git commit, git diff, git reset, git restore, git push |
| Level 33 | Bash special variables, shell environments                  |                                                                 |
