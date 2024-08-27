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
ssh bandit14@localhost -i sshkey.private -p 2220

cat /etc/bandit_pass/bandit14
> **redacted**
```

## Level 14 -> 15
### Level Goal
The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.
### Walkthrough

```bash
nc -N localhost 30000
{PASSWORD OF LEVEL 14}
> Correct!
> **redacted**
```

## Level 15 -> 16
### Level Goal
The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL/TLS encryption.
### Walkthrough

```bash
openssl s_client -connect localhost:30001

> CONNECTED(00000003)
> Cannot use SSL_get_servername
> depth=0 CN = SnakeOil
> ...
> read R BLOCK

{PASSWORD OF LEVEL 15}
> Correct!
> **redacted**
```

## Level 16 -> 17
### Level Goal
The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
### Walkthrough

```bash
nmap -sV -sC -p 31000-32000 localhost

> Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-08-27 12:03 UTC
> Nmap scan report for localhost (127.0.0.1)
> Host is up (0.00020s latency).
> Not shown: 996 closed tcp ports (conn-refused)
> PORT      STATE SERVICE     VERSION
> 31046/tcp open  echo
> 31518/tcp open  ssl/echo
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
> 31691/tcp open  echo
> 31790/tcp open  ssl/unknown
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=SnakeOil
| Not valid before: 2024-06-10T03:59:50
|_Not valid after:  2034-06-08T03:59:50
| fingerprint-strings:
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, LPDString, RTSPRequest, SIPOptions:
|_    Wrong! Please enter the correct current password.
> 31960/tcp open  echo
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31790-TCP:V=7.94SVN%T=SSL%I=7%D=8/27%Time=66CDC0AD%P=x86_64-pc-linu
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

openssl s_client -ign_eof -connect localhost:31790
> Correct!
**redacted**
```

## Level 17 -> 18
### Level Goal
There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**
### Walkthrough

```bash
ssh -i {RSA-PKEY of Level 16} bandit17@bandit.labs.overthewire.org

diff passwords.new passwords.new
> < bSrACvJvvBSxEM2SGsV5sn09vc3xgqyp
> ---
> **redacted**
```

## Level 18 -> 19
### Level Goal
he password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.
### Walkthrough

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "cat readme"
> **redacted**
```

## Level 19 -> 20
### Level Goal
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
### Walkthrough

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
> **redacted**
```

## Level 20 -> 21
### Level Goal
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
### Walkthrough

```bash
nc -lvnp 10001
> Listening on 0.0.0.0 10001
> Connection received on 127.0.0.1 

{PASSWORD OF LEVEL 20}
> **redacted
```

```bash
./suconnect 10001
```

## Level 21 -> 22
### Level Goal
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
### Walkthrough

```bash
ls /etc/cron.d
> cronjob_bandit22 ...

cat /etc/cron.d/cronjob_bandit22
> * * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null

cat /usr/bin/cronjob_bandit22.sh
> #!/bin/bash
> chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
> cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
> **redacted**
```

## Level 22 -> 23
### Level Goal
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
### Walkthrough

```bash
cat /etc/cron.d/cronjob_bandit23
> @reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
> * * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null

cat /usr/bin/cronjob_bandit23.sh
> #!/bin/bash

> myname=$(whoami)
> mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

> echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

> cat /etc/bandit_pass/$myname > /tmp/$mytarget

echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
> 8ca319486bfbbc3663ea0fbe81326349

cat /tmp/8ca319486bfbbc3663ea0fbe81326349
> **redacted**
```

## Level 23 -> 24
### Level Goal
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
### Walkthrough

```bash
cat /usr/bin/cronjob_bandit24.sh
> #!/bin/bash

> myname=$(whoami)

> cd /var/spool/$myname/foo
> echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
> for i in * .*;
> do
>   if [ "$i" != "." -a "$i" != ".." ];
>    then
>        echo "Handling $i"
>        owner="$(stat --format "%U" ./$i)"
>        if [ "${owner}" = "bandit23" ]; then
>            timeout -s 9 60 ./$i
>        fi
>        rm -f ./$i
>    fi
> done

mktemp -d scriptXXX

cd tmp/scriptXXX

chmod 777 /tmp/scriptXXX
```

```bash
nano getpass.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/scriptXXX/pass
```

```bash
chmod 777 getpass.sh

cp getpass.sh /var/spool/bandit24/foo/getpass.sh

cat pass
> **redacted**
```

## Level 24 -> 25
### Level Goal
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
### Walkthrough

```bash
nano bruteforcer.sh
#!/bin/bash

# The base string you want to combine with the 4-digit code
base_string="{PASSWORD OF LEVEL 24} " # Make sure you include a space after the password!

# The target host and port to connect to
host="localhost"
port="30002"

# Establish a connection and keep it open
# The file descriptor 3 is used to keep the connection open
exec 3<>/dev/tcp/$host/$port

# Check if the connection was successful
if [ $? -ne 0 ]; then
    echo "Failed to connect to $host on port $port"
    exit 1
fi

# Loop through all numbers from 0000 to 9999
for i in $(seq -w 0000 9999); do
    # Combine the base string with the 4-digit code
    combined_string="${base_string}${i}"

    # Send the combined string to the server
    echo "$combined_string" >&3

    # Read the response from the server
    read -u 3 response

    # Print the combined string and the response
    echo "Sent: $combined_string, Received: $response"

    # Optional: Stop if a certain response is received
    # if [[ "$response" == "ExpectedResponse" ]]; then
    #     echo "Stopping as the expected response was received."
    #     break
    # fi
done

# Close the connection
exec 3<&-
exec 3>&-
```

```bash
./brutefocer.sh
...
Sent: **redacted** 3947, Received: Wrong! Please enter the correct current password and pincode. Try again.
Sent: **redacted** 3948, Received: Correct!
Sent: **redacted** 3949, Received: The password of user bandit25 is **redacted**
```

## Level 25 -> 26
### Level Goal
Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not **/bin/bash**, but something else. Find out what it is, how it works and how to break out of it.
### Walkthrough

```
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

Resize the window to enter more mode:

```bash
v

:set shell=/bin/bash
:shell

cat /etc/bandit_pass/bandit26
> **redacted**
```

## Level 26 -> 27
### Level Goal
Good job getting a shell! Now hurry and grab the password for bandit27!
### Walkthrough

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
> **redacted**
```

## Level 27 -> 28
### Level Goal
There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.
### Walkthrough

```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo

cat /repo/README
> The password to the next level is: **redacted**
```

## Level 28 -> 29
### Level Goal
There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

Clone the repository and find the password for the next level.
### Walkthrough

```bash
git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo

cat /repo/README.md

git log
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    fix info leak

commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

commit 5f7265568c7b503b276ec20f677b68c92b43b712
Author: Ben Dover <noone@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    initial commit of README.md


git show 73f5d0435070c8922da12177dc93f40b2285e22a
commit 73f5d0435070c8922da12177dc93f40b2285e22a
Author: Morla Porla <morla@overthewire.org>
Date:   Wed Jul 17 15:57:30 2024 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: <TBD>
+- password: **redacted**
```

## Level 29 -> 30
### Level Goal
There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

Clone the repository and find the password for the next level.
### Walkthrough

