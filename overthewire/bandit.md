# bandit
- [game link](https://overthewire.org/wargames/bandit/)
- ssh command: `ssh bandit0@bandit.labs.overthewire.org -p 2220`
  - NOTE: change number in username to level number

## level 0
- ssh into machine with password bandit0 (this password is given on website. no more passwords will be logged in this writeup)
- run `ls -la`
- password is in readme (view with cat or vim)

## level 1
- run `ls -la`
- run `vim .` and navigate to the file named "-" where the password is stored

## level 2
- run `ls -la`
- run vim on cwd then select file or merely run `cat ./--spaces\ in\ this\ filename--`

## level 3
- cd into visible directory
- cat hidden file

## level 4
- cd into visible directory
- run `file ./*` to see the file type of all the files
- cat the file with human readable type (ASCII text)

## level 5
- `cd inhere`
- Check for non executable files of 1033 bytes: `find . -type f -size 1033c ! -executable`
- Cat only returned file

## level 6
- need to search entire file system for file with multiple attributes (specific user, group, and size)
- run `find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null`
- NOTE: command returns many files with errors like "Permission denied" so it's best to redirect standard error to /dev/null (which disgards them)
