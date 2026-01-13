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
