# Bandit
[Bandit wargame](https://overthewire.org/wargames/bandit/) is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames.

If you like OverTheWire wargames please consider [donating to them](https://overthewire.org/information/donate.html) to support the project.
## Table of Contents
- [[#Level 0 -> Level 1|Level 0 -> Level 1]]
	- [[#Level 0 -> Level 1#Level Goal|Level Goal]]
	- [[#Level 0 -> Level 1#Walkthrough|Walkthrough]]
- [[#Level 1 -> 2|Level 1 -> 2]]
	- [[#Level 1 -> 2#Level Goal|Level Goal]]
	- [[#Level 1 -> 2#Walkthrough|Walkthrough]]
- [[#Level 2 -> 3|Level 2 -> 3]]
	- [[#Level 2 -> 3#Level Goal|Level Goal]]
	- [[#Level 2 -> 3#Walkthrough|Walkthrough]]
- [[#Level 3 -> 4|Level 3 -> 4]]
	- [[#Level 3 -> 4#Level Goal|Level Goal]]
	- [[#Level 3 -> 4#Walkthrough|Walkthrough]]
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