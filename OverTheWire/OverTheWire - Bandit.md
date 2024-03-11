#linuxcmd 
*Complete entire game to master the linux/unix CMI* [here](https://overthewire.org/wargames/bandit/)
1. If you are stuck, avoid immediately looking up the solution but instead research on the techniques that might help you solve the level.
2. ***Commands you may need to solve this level*** contains important commands that you should learn about/know. Look them all up if you can.
```cmd
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

> [!summary]- Table of Contents
> ```table-of-contents
style: nestedList # TOC style (nestedList|inlineFirstLevel)
minLevel: 0 # Include headings from the specified level
maxLevel: 1 # Include headings up to the specified level
includeLinks: true # Make headings clickable
debugInConsole: false # Print debug info in Obsidian console```
# Level 0-1
use common sense
# Level 1-2
**Problem**
The password for the next level is stored in a file called **-** located in the home directory

**Solution**
```cmd
cat ./-
```
# Level 2-3
use common sense
# Level 3-4
**Problem**
The password for the next level is stored in a hidden file in the **inhere** directory.

**Solution**
1. List all the files in the directory with [[ls]]
```cmd
ls -la

total 12
drwxr-xr-x 2 root    root    4096 Oct  5 06:19 **.**
drwxr-xr-x 3 root    root    4096 Oct  5 06:19 **..**
-rw-r----- 1 bandit4 bandit3   33 Oct  5 06:19 .hidden
```
2. ```cat .hidden```
# Level 4-5
**Problem**
The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

**Solution**
1. ```cd inhere```
2. Listing the files, we get:
```cmd
ls

-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```
3. Running the [[file]] command on a file returns the file type and from there we can deduce the only human-readable file in the directory. 
4. However it's tedious to run the command on every single file, plus we run into the problem of file starting with ```-```. We use the technique in [[OverTheWire - Bandit#Level 1-2|Level 1-2]] to tackle this. 
5. ```file *``` would list down the types of all files in the directory and combining the technique above:
```cmd
file ./*

./-file00: data
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
6. ```cat ./-file07```
# Level 5-6
**Problem**
The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

**Solution**
1. You could search for the file with separate conditions but it would be best if we can find a one line command that would only give return us one file. For this level, just searching for the file with the correct size will do
```cmd
du -a -b | grep 1033

1033	./maybehere07/.file2
```
However, what if there are multiple files with the same size? 

2. [[find]] will list all files in a directory. There should also be enough options to combine the conditions
```cmd
find . -type f -size 1033c ! -executable -exec file '{}' \; | grep ASCII

./maybehere07/.file2: ASCII text, with very long lines (1000)
```
```find .```: find in the current working directory,
```-type f```: the regular files (as opposed to directories, symbolic links, etc.) that are
```-size 1033c```: 1033 bytes in size and 
```! -executable```: are not executable,
```-exec file '{}' \;```: run the ```file``` command on every file found, ```{}``` represents the filename found by ```find```, ```\;``` marks the end of the command to be executed
```| grep ASCII```: filter to display only those are human-readable (ASCII)

learn about pipes (```|```) [[Linux CMI#pipes|here]]
# Level 6-7
**Problem**
The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

**Solution**
1. we should start searching for files from the root directory (somewhere on the server), [[Linux CMI#find|find]] ```/```
2. to find files owned by user bandit7, ```-user bandit7```
3. to find files owned by group bandit6, ```-group bandit6```
4. 33 bytes in size, ```-size 33c```
5. hide ```Permission Denied``` errors^[https://stackoverflow.com/questions/762348/how-can-i-exclude-all-permission-denied-messages-from-find], ```2>/dev/null```: basically dumps all errors into the /dev/null directory
6. Together,
```cmd
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null

/var/lib/dpkg/info/bandit7.password
```
# Level 7-8
**Problem**
The password for the next level is stored in the file **data.txt** next to the word **millionth**

**Solution**
1. use ```grep``` to find the word millionth
```cmd
grep "millionth" data.txt
millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
# Level 8-9
**Problem**
The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

**Solution**
1. the ```sort``` command sorts the lines in a text file alphabetically by default
2. the ```uniq``` command filters out duplicate lines from the sorted input it received, ```-u``` tells it to only display the unique lines
```cmd
sort data.txt | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```
# Level 9-10
**Problem**
The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

**Solution**
1. ```strings``` command extracts all readable strings from a file
2. ```grep``` can be used to find the several = characters
```cmd
strings data.txt | grep ===

x]T========== theG)"
========== passwordk^
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

# Level 10-11
**Problem**
The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

**Solution**
1. you can use online tools such as CyberChef to decode the base64 data
2. or you can use built-in base64 command line tools
```cmd
base64 -d data.txt

The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
# Level 11-12
**Problem**
The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

**Solution**
1. this is a simple caesar cipher, [ROT13](https://en.wikipedia.org/wiki/ROT13)
2. you can use online tools such as CyberChef to decrypt the cipher but there is also a command line solution with the ```tr``` command^[https://unix.stackexchange.com/questions/19772/how-does-tr-a-z-n-za-m-work]
```cmd
cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"

The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
# Level 12-13
**Problem**
The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

**Solution**
# Level 13-14
**Problem**
The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on
**Solution**
# Level 14-15
**Problem**
The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.
**Solution**
# Level 15-16
**Problem**
The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.
**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**
**Solution**

# Level 16-17
**Problem**
The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
**Solution**

# Level 17-18
**Problem**
There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**
**Solution**
# Level 18-19
**Problem**
The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.
**Solution**
# Level 19-20
**Problem**
To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
**Solution**
# Level 20-21
**Problem**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

**Solution**
# Level 21-22
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
**Solution**
# Level 22-23
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.
**Solution**
# Level 23-24
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…
**Solution**
# Level 24-25
**Problem**
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time

**Solution**
# Level 25-26
**Problem**
**Solution**
# Level 26-27
**Problem**
**Solution**
