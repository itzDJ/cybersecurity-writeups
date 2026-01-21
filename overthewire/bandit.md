# bandit
- [game link](https://overthewire.org/wargames/bandit/)
- ssh command: `ssh bandit0@bandit.labs.overthewire.org -p 2220`
  - NOTE: change number in username to level number

## level 0
### problem
The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.
### solution
- ssh into machine with password bandit0 (this password is given on website. no more passwords will be logged in this writeup)
- run `ls -la`
- password is in readme (view with cat or vim)

## level 1
### problem
The password for the next level is stored in a file called - located in the home directory
### solution
- run `ls -la`
- run `vim .` and navigate to the file named "-" where the password is stored

## level 2
### problem
The password for the next level is stored in a file called --spaces in this filename-- located in the home directory
### solution
- run `ls -la`
- run vim on cwd then select file or merely run `cat ./--spaces\ in\ this\ filename--`

## level 3
### problem
The password for the next level is stored in a hidden file in the inhere directory.
### solution
- cd into visible directory
- cat hidden file

## level 4
### problem
The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.
### solution
- cd into visible directory
- run `file ./*` to see the file type of all the files
- cat the file with human readable type (ASCII text)

## level 5
### problem
The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
- human-readable
- 1033 bytes in size
- not executable
### solution
- `cd inhere`
- Check for non executable files of 1033 bytes: `find . -type f -size 1033c ! -executable`
- Cat only returned file

## level 6
### problem
The password for the next level is stored somewhere on the server and has all of the following properties:
- owned by user bandit7
- owned by group bandit6
- 33 bytes in size
### solution
- need to search entire file system for file with multiple attributes (specific user, group, and size)
- run `find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null`
- NOTE: command returns many files with errors like "Permission denied" so it's best to redirect standard error to /dev/null (which disgards them)
- Cat the only revealed file

## level 7
### problem
The password for the next level is stored in the file data.txt next to the word millionth
### solution
- password is next to word millionth
- run `cat data.txt | grep millionth`

## level 8
### problem
The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
### solution
- uniq command compares adjacent lines and the -u flag checks that each line is followed by a unique line
- because subsequent lines are checked, the data must be sorted first
- run `sort data.txt | uniq -u`

## level 9
### problem
The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
### solution
- strings command can find human readable strings in binary files
- run `strings data.txt | grep =`
  - NOTE: if several equals are used instead of one as per the instructions, the password is even more apparent
- password is one of relatively few returned lines (preceded by a bunch of equal signs)

## level 10
### problem
The password for the next level is stored in the file data.txt, which contains base64 encoded data
### solution
- check help page for base64
- peep -d flag to decode file
- run `base64 -d data.txt`

## level 11
### problem
The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
### solution
- found solution in bash as well as python
- rot13 in bash: `cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"`
- rot13 in python3 REPL
```python
import codecs
with open("data.txt") as f:
    codecs.decode(f.read(), "rot_13")
```

## level 12
### problem
The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)
### solution
- based on rules of the game and permissions on the filesystem, files cannot be created and removed just anywhere on the device
- run `mktemp -d` to create a temporary directory and change into it
- run `xxd -r ~/data.txt data` to reverse the hex dump located in ~/data.txt and create a binary file from it called data in our temp dir
- run the file command on data and decompress based on file type
  - if gzip, use mv to rename with .gz then run `gzip -d`
  - if bzip2, run `bzip2 -d`
  - if tar, run `tar -xf`
- continue that process until the file type is ASCII text then cat the file
- cleanup any temp files

## level 13
### problem
The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.
### solution
- copy contents of ssh key located in home directory of bandit13 to somewhere on our host machine (maybe to file named `ssh_key`)
- increase permission strictness on the private key: `chmod 600 ssh_key`
- ssh into next level with the key: `ssh -i ssh_key bandit14@bandit.labs.overthewire.org -p 2220`

## level 14
### problem
The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
### solution
- After signed into level from ssh private key, `cat /etc/bandit_pass/bandit14` as mentioned in previous level
- Send that password to port 30000 on localhost using netcat: `cat /etc/bandit_pass/bandit14 | nc localhost 30000`
- Password for next level is sent in return

## level 15
### problem
The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
### solution
- use nmap's ncat instead of nc for easier ssl connection
- run: `cat /etc/bandit_pass/bandit15 | ncat --ssl localhost 30001`

## level 16
### problem
The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
### solution
- find open ports from 31000 to 32000: `nmap -p 31000-32000 localhost`
- connect to the returned ports one by one: `openssl s_client -connect localhost:<port> -quiet`
- if a connection is made, paste the password for current level and click enter
- repeat until an RSA private key is returned
- save that key to host machine and modify permissions to `chmod 600` just like level 13

## level 17
### problem
There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
### solution
- run: `diff passwords.old passwords.new`
- output shows the files only differ on one line
- first string is from old file; second string is from new file (second is password for next level)

## level 18
### problem
The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
### solution
- need to get contents of readme file before .bashrc logs out
- execute a single command over ssh (since it's non-interactive, .bashrc never loads)
- `ssh bandit18@bandit.labs.overthewire.org -p 2220 'cat readme'`
