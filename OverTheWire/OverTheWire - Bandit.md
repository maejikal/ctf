*Complete entire game to master the linux/unix CMI* [here](https://overthewire.org/wargames/bandit/)
1. If you are stuck, avoid immediately looking up the solution but instead research on the techniques that might help you solve the level.
2. ***Commands you may need to solve this*** contains important commands that you should learn about/know. Look them all up if you can.
```cmd
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
<br>
*not finished*


[ 0-1](#0-1) | [ 1-2](#1-2) |
[ 2-3](#2-3) |
[ 3-4](#3-4) |
[ 4-5](#4-5) |
[ 5-6](#5-6) |
[ 6-7](#6-7) |
[ 7-8](#7-8) |
[ 8-9](#8-9) |
[ 9-10](#9-10) |
[ 10-11](#10-11) |
[ 11-12](#11-12) |
[ 12-13](#12-13) |
[ 13-14](#13-14) |
[ 14-15](#14-15) |
[ 15-16](#15-16) |
[ 16-17](#16-17) |
[ 17-18](#17-18) |
[ 18-19](#18-19) |
[ 19-20](#19-20) |
[ 20-21](#20-21) |
[ 21-22](#21-22) |
[ 22-23](#22-23) |
[ 23-24](#23-24) |
[ 24-25](#23-24) |
[ 25-26](#23-24) |
[ 26-27](#23-24) |
[ 27-28](#23-24) |
[ 28-29](#23-24) |
[ 29-30](#23-24) |
[ 30-31](#23-24) | [ 31-32](#23-24) | [ 32-33](#23-24) |
[ 33-34](#23-24) |

# 0-1
use common sense
# 1-2
**Problem**
The password for the next  is stored in a file called **-** located in the home directory

**Solution**
```cmd
cat ./-
```
# 2-3
use common sense
# 3-4
**Problem**
The password for the next  is stored in a hidden file in the **inhere** directory.

**Solution**
1. List all the files in the directory with ```ls```
```cmd
ls -la

total 12
drwxr-xr-x 2 root    root    4096 Oct  5 06:19 **.**
drwxr-xr-x 3 root    root    4096 Oct  5 06:19 **..**
-rw-r----- 1 bandit4 bandit3   33 Oct  5 06:19 .hidden
```
2. ```cat .hidden```
# 4-5
**Problem**
The password for the next  is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

**Solution**
1. ```cd inhere```
2. Listing the files, we get:
```cmd
ls

-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```
3. Running the ```file``` command on a file returns the file type and from there we can deduce the only human-readable file in the directory. 
4. However it's tedious to run the command on every single file, plus we run into the problem of file starting with ```-```. We use the technique in [1-2](#1-2) to tackle this. 
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
# 5-6
**Problem**
The password for the next  is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

**Solution**
1. You could search for the file with separate conditions but it would be best if we can find a one line command that would only give return us one file. For this , just searching for the file with the correct size will do
```cmd
du -a -b  | grep 1033

1033	./maybehere07/.file2
```
However, what if there are multiple files with the same size? 

2. ```find``` will list all files in a directory. There should also be enough options to combine the conditions
```cmd
find . -type f -size 1033c ! -executable -exec file '{}' \;  | grep ASCII

./maybehere07/.file2: ASCII text, with very long lines (1000)
```
```find .```: find in the current working directory, <br>
```-type f```: the regular files (as opposed to directories, symbolic links, etc.) that are <br>
```-size 1033c```: 1033 bytes in size and <br>
```! -executable```: are not executable, <br>
```-exec file '{}' \;```: run the ```file``` command on every file found, <br>```{}``` represents the filename found by ```find```, ```\;``` marks the end of the command to be executed<br>
``` | grep ASCII```: filter to display only those are human-readable (ASCII)

# 6-7
**Problem**
The password for the next  is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

**Solution**
1. we should start searching for files from the root directory (somewhere on the server), ```find /```
2. to find files owned by user bandit7, ```-user bandit7```
3. to find files owned by group bandit6, ```-group bandit6```
4. 33 bytes in size, ```-size 33c```
5. hide ```Permission Denied``` errors, ```2>/dev/null```: basically dumps all errors into the /dev/null directory
6. Together,
```cmd
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null

/var/lib/dpkg/info/bandit7.password
```
# 7-8
**Problem**
The password for the next  is stored in the file **data.txt** next to the word **millionth**

**Solution**
1. use ```grep``` to find the word millionth
```cmd
grep "millionth" data.txt
millionth	TESKZC0XvTetK0S9xNwm25STk5iWrBvP
```
# 8-9
**Problem**
The password for the next  is stored in the file **data.txt** and is the only line of text that occurs only once

**Solution**
1. the ```sort``` command sorts the lines in a text file alphabetically by default
2. the ```uniq``` command filters out duplicate lines from the sorted input it received, ```-u``` tells it to only display the unique lines
```cmd
sort data.txt  | uniq -u
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```
#  9-10
**Problem**
The password for the next  is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

**Solution**
1. ```strings``` command extracts all readable strings from a file
2. ```grep``` can be used to find the several = characters
```cmd
strings data.txt  | grep ===

x]T========== theG)"
========== passwordk^
========== is
========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s
```

#  10-11
**Problem**
The password for the next  is stored in the file **data.txt**, which contains base64 encoded data

**Solution**
1. you can use online tools such as CyberChef to decode the base64 data
2. or you can use built-in base64 command line tools
```cmd
base64 -d data.txt

The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM
```
#  11-12
**Problem**
The password for the next  is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

**Solution**
1. this is a simple caesar cipher, [ROT13](https://en.wikipedia.org/wiki/ROT13)
2. you can use online tools such as CyberChef to decrypt the cipher but there is also a command line solution with the ```tr``` command
```cmd
cat data.txt  | tr "A-Za-z" "N-ZA-Mn-za-m"

The password is JVNBBFSmZwKKOP0XbFXOoW8chDz5yVRv
```
#  12-13
**Problem**
The password for the next  is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this  it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

**Solution**
1. Create a directory to work on the file since we do not have permissions to work in the root directory.
```cmd
mktemp -d
/tmp/tmp.sc0kPF5eiy
```
2. Copy ```data.txt``` into ```/tmp/tmp.sc0kPF5eiy```
```cmd
cd /tmp/tmp.sc0kPF5eiy
cp ~/data.txt .
```
3. Reverse hexdump (data.txt) and output it into a file
```cmd
cat data.txt | xxd -r > compressed_data
```
```xxd```: tool used for generating/reversing hex dumps

4. Running ```file``` on compressed_data,
```cmd
file compressed_data

compressed_data: gzip compressed data, was "data2.bin", last modified: Thu Oct  5 06:19:20 2023, max compression, from Unix, original size modulo 2^32 573
```
5. Rename the file to its respective extension, decompress the gzip file
```cmd
mv compressed_data compressed.gz
gzip -d compressed.gz
```
6. Repeat 4-5, checking the type of compressed data and use the correct method of decompressing until you get an ASCII text file
#  13-14
**Problem**
The password for the next  is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this , you don’t get the next password, but you get a private SSH key that can be used to log into the next . **Note:** **localhost** is a hostname that refers to the machine you are working on
**Solution** <br>
There are 2 solutions to solve this:<br>
**A.**
1. We are given a private ssh key, what we need to do is extract it into our local machine (own computer, not the server machine) and then use the credentials to log into the next level
2. ```scp``` can be used to transfer files between computers, run this command from your own terminal/command prompt (disconnected from game server)
```cmd
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```
3. Enter the password to complete the download
4. Log in
```cmd
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

You will get a permission error:
```
Permissions 0640 for 'sshkey.private' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```

5. Modify the key permissions such that only you can access it
```cmd
chmod 700 sshkey.private
```
6. Step 4

**B.**
1. Since you are already logged into bandit13, you can ssh to bandit14 directly via localhost without disconnecting from the game server. 
```cmd
ssh -i sshkey.private bandit14@localhost -p 2220
```
#  14-15
**Problem**
The password for the next  can be retrieved by submitting the password of the current  to **port 30000 on localhost**.
**Solution**
1. Use ```nc``` to connect to port 30000
```cmd
nc localhost 30000
```
2. Enter the password found in previous level
#  15-16
**Problem**
The password for the next  can be retrieved by submitting the password of the current  to **port 30001 on localhost** using SSL encryption.
**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**
**Solution**
1. ```openssl``` is the foundation of secure communications on the internet (https) 
```cmd
openssl s_client -connect localhost:30001

CONNECTED(00000003)
Can't use SSL_get_servername
...
---
read R BLOCK
```
2. After running the above, there will be a prompt on the line after read R BLOCK, paste the password and you will be given the password for the next level
#  16-17
**Problem**
The credentials for the next  can be retrieved by submitting the password of the current  to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
**Solution**
1. To scan for an open port, use ```nmap```. Checking the help page, 
```cmd
nmap -h

Usage: nmap [Scan Type(s)] [Options] {target specification}
...
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
...
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
    ---
```
2. Since the port is on localhost and in the range 31000-32000, follow the usage guide above
```cmd
nmap -sV localhost -p31000-32000

PORT      STATE SERVICE     VERSION
31046/tcp open  echo
31518/tcp open  ssl/echo
31691/tcp open  echo
31790/tcp open  ssl/unknown
31960/tcp open  echo
```
3. Only port 31790 doesn't echo back whatever you send it, seems promising
```cmd
openssl s_client localhost:31790

CONNECTED(00000003)
Can't use SSL_get_servername
...
---
read R BLOCK
```
4. Enter the password for current level
#  17-18
**Problem**
There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next  is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this  and see ‘Byebye!’ when trying to log into bandit18, this is related to the next , bandit19**
**Solution**
#  18-19
**Problem**
The password for the next  is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.
**Solution**
#  19-20
**Problem**
To gain access to the next , you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this  can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
**Solution**
#  20-21
**Problem**
There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous  (bandit20). If the password is correct, it will transmit the password for the next  (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think

**Solution**
#  21-22
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.
**Solution**
#  22-23
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this  is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.
**Solution**
#  23-24
**Problem**
A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This  requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this !

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…
**Solution**
# 24-25
**Problem**
A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.  
You do not need to create new connections each time

**Solution**
# 25-26
**Problem**
**Solution**
# 26-27
**Problem**
**Solution**
