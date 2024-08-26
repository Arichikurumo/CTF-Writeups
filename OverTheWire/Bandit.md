# Bandit
[Bandit wargame](https://overthewire.org/wargames/bandit/) is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames.

If you like OverTheWire wargames please consider [donating to them](https://overthewire.org/information/donate.html) to support the project.
## Level 0 -> Level 1
### Level Goal
The password for the next level is stored in a file called **readme** located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

### Walkthrough
Start by connecting to the machine via ssh. Use the password mentioned in the task:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

You should then be connected to the machine. To read the ‘readme’ file, we first check whether it is available in the current directory using _ls_:

```bash
ls
> readme
```

To read the content of the file use _cat_:

```bash
cat
> Congratulations on your first steps into the bandit game!!
> Please make sure you have read the rules at https://overthewire.org/rules/
> If you are following a course, workshop, walthrough or other educational 
> activity,
> please inform the instructor about the rules as well and encourage them to
> contribute to the OverTheWire community so we can keep these games free!

> The password you are looking for is: **redacted**
```

## Level 1 -> 2
### Level Goal
The password for the next level is stored in a file called **-** located in the home directory.
### Walkthrough

```bash
cat ./-
```

## Level 2 -> 3
### Level Goal
The password for the next level is stored in a file called **spaces in this filename** located in the home directory.
### Walkthrough

```bash
 cat "spaces in this filename"
```

## Level 3 -> 4
### Level Goal
The password for the next level is stored in a hidden file in the **inhere** directory.
### Walkthrough

```bash
cd ./inhere

cat ...Hiding-From-You
```

## Level 4 -> 5
### Level Goal
The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.
### Walkthrough

```bash
file -- -file00
> data
..
file -- -file07
> ASCII text

cat -- -file007
```

or a more elegant solution:

```bash
find . -type f -exec file {} \; | grep -i "ascii text" | cut -d: -f1 | xargs cat
```

## Level 5 -> 6
### Level Goal
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable
### Walkthrough

```bash
find . -type f -size 1033c ! -executable -exec file {} \; | grep -i text
> ./maybehere07/.file2
```

## Level 6 -> 7
### Level Goal
The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size
### Walkthrough

```bash
find / -group "bandit6" -user "bandit7" -size 33c 2>/dev/null
> /var/lib/dpkg/info/bandit7.password
```
## Level 7 -> 8
### Level Goal
The password for the next level is stored in the file **data.txt** next to the word **millionth**.
### Walkthrough

```bash
cat data.txt | grep -i "millionth"
> millionth      **redacted**
```

## Level 8 -> 9
### Level Goal
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once.
### Walkthrough

```bash
sort data.txt | uniq -u
> **redacted**
```

## Level 9 -> 10
### Level Goal
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.
### Walkthrough

```bash
strings data.txt | grep -E "===*"
> \a!;========== the
> ========== passwordf
> ========== isc
> ========== **redacted**
```

## Level 10 -> 11
### Level Goal
The password for the next level is stored in the file **data.txt**, which contains base64 encoded data.
### Walkthrough

```bash
base64 -d data.txt
> The password is **redacted**
```

## Level 11 -> 12
### Level Goal
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.
### Walkthrough

```bash
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
> The password is **redacted**
```
 `tr 'A-Za-z' 'N-ZA-Mn-za-m'`: The `tr` command is used to translate or delete characters. Here, it translates all uppercase and lowercase letters from 'A-Z' to 'N-Z' and 'A-M', and from 'a-z' to 'n-z' and 'a-m'. This effectively performs the ROT13 transformation.

## Level 12 -> 13
### Level Goal
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)
### Walkthrough

```bash
xxd -r data.txt output.bin

file output.bin
> output.bin: gzip compressed data

gzip -d --suffix .bin output.bin

file output
> output: bzip2 compressed data, block size = 900k

bzip2 -d output

file output.out
> output.out: gzip compressed data

gzip -d --suffix .out output.out

file output
> output: POSIX tar archive (GNU)

tar -xf output

file data5.bin
> data5.bin: POSIX tar archive (GNU)

tar -xf data5.bin

file data6.bin
> data6.bin: bzip2 compressed data, block size = 900k

bzip2 -d data6.bin

file data6.bin.out
> data6.bin.out: POSIX tar archive (GNU)

tar -xf data6.bin.out

file data8.bin
> data8.bin: gzip compressed data

gzip -d --suffix .bin data8.bin

cat data8
> The password is **redacted**
```

## Level 13 -> 14
### Level Goal
The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on.
### Walkthrough

```bash
ssh bandit14@localhost -i sshkey.private

cat /etc/bandit_pass/bandit14
> **redacted**
```