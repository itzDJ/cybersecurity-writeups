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
