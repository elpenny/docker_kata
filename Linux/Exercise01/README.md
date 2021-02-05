# Exercise 1 - Welcome to UNIX-like systems

This is first exercise of new series: getting into Linux. In this exercise we will learn basics of Linux, shells and terminal navigation

To run this exercise you need to have:
1. Windows 10 with WSL2 enabled. You can try with Hyper-V but performance will be somewhat limited. In order to install WSL2, please refer to [this](https://docs.microsoft.com/en-us/windows/wsl/install-win10), in order to check if its enabled, go [here](https://superuser.com/questions/1551146/how-to-verify-that-wsl-2-is-used).

OR

2. working Linux distribution


## Kata
1. Open up your Linux.
2. Type ```ps | grep $$```
3. Type ```which bash```
4. You just used 3 different shell programs (ps, grep and which), special `$$` variable and pipe `|` operator. Ps (process status) displays currently running processes, grep is used to make search, which shows you location of program, `$$` contains PID of shell process and pipe is used to direct output of one program as inpout of second. To learn more of any program you should use manpages.
5. Type `man which` to see manpages for `which`.
6. On shell variables: Shell variables are created once they are assigned a value. A variable can contain a number, a character or a string of characters. Variable name is case sensitive and can consist of a combination of letters and the underscore "_". Value assignment is done using the "=" sign. Note that no space permitted on either side of = sign when initializing variables.
7. Let's test this knowledge, type 
```bash 
PRICE_PER_APPLE=5;
echo "The price of an Apple today is: $PRICE_PER_APPLE"
echo "The price of an Apple today is: ${PRICE_PER_APPLE}"
```
8. Variables can be assigned with the value of a command output. This is referred to as substitution. Substitution can be done by encapsulating the command with `` (known as back-ticks) or with $()
9. Type 
```bash
FileWithTimeStamp=/tmp/my-dir/file_$(/bin/date +%Y-%m-%d).txt
echo $FileWithTimeStamp
```
10. Good enough! You just learnt new command: `date`. Now, to test all this knowledge try to complete this short exercise:
```bash
The target of this exercise is to create a string, an integer, and a complex variable using command substitution. The string should be named BIRTHDATE and should contain the text "Jan 1, 2000". The integer should be named Presents and should contain the number 10. The complex variable should be named BIRTHDAY and should contain the full weekday name of the day matching the date in variable BIRTHDATE e.g. Saturday. Note that the 'date' command can be used to convert a date format into a different date format. For example, to convert date value, $date1, to day of the week of date1, use:

date -d "$date1" +%A
```

11. In this directory there is file ex1.sh, this is script for validating your answer. Edit this file to perform exercise and launch it by typing `./ex1.sh` from terminal in current dir.
12. Anatomy of Bash script:
    - shebang (NOT A COMMENT)
    - flow control (IF, ELSE, THEN, FI)

13. If script wont start up you might need to make it executable, run this command: ```chmod +x ./ex1.sh ```
14. If this wont help, you need to elevate yourself as a SuperUser, p[repend `sudo` to you previous command to do this. (On WSL you might not know you password, look here:
https://docs.microsoft.com/en-us/windows/wsl/user-support)
15. You now have very basic understanding of bash, its scripts and variables, also you learnt some popular linux commands.


