# Exercise 2 - Welcome to UNIX-like systems, part 2

This is second exercise of new series: getting into Linux. In this exercise we will learn basics of Linux, shells and terminal navigation

To run this exercise you need to have:
1. Windows 10 with WSL2 enabled. You can try with Hyper-V but performance will be somewhat limited. In order to install WSL2, please refer to [this](https://docs.microsoft.com/en-us/windows/wsl/install-win10), in order to check if its enabled, go [here](https://superuser.com/questions/1551146/how-to-verify-that-wsl-2-is-used).

OR

2. working Linux distribution


## Kata
1. Open up your Linux.
2. Use 'cat' to print content of `ex01.sh` to console.
3. Script is very simple, It only demonstrates how to pass args to script.
4. Type `./ex01.sh Hello World` and observe what is happening.
5. Type `./ex01.sh "Hello World" "From bash"` and observe what is happening.
6. Type `./ex01.sh "Hello World" "From Bash" "with love"` and observe what is happening.
7. Type `./ex02.sh` and observe output. Special Variable `$0` denotes name of the script or shell. Try running `echo $0` in terminal directly and see what happens.
8. Now for arrays: An array can hold several values under one name. Array naming is the same as variables naming. An array is initialized by assign space-delimited values enclosed in ()
```bash
my_array=(apple banana "Fruit Basket" orange)
new_array[2]=apricot
```
9. Array members need not be consecutive or contiguous. Some members of the array can be left uninitialized. The total number of elements in the array is referenced by `${#arrayname[@]}`
```bash
my_array=(apple banana "Fruit Basket" orange)
echo  ${#my_array[@]}                   # 4
```
10. The array elements can be accessed with their numeric index. The index of the first element is 0.
```bash
my_array=(apple banana "Fruit Basket" orange)
echo ${my_array[3]}                     # orange - note that curly brackets are needed
# adding another array element
my_array[4]="carrot"                    # value assignment without a $ and curly brackets
echo ${#my_array[@]}                    # 5
echo ${my_array[${#my_array[@]}-1]}     # carrot
```
11. Exercise:
```
In this exercise, you will need to add numbers and strings to the correct arrays. You must add the numbers 1,2, and 3 to the "numbers" array, and the words 'hello' and 'world' to the strings array.

You will also have to correct the values of the variable NumberOfNames and the variable second_name. NumberOfNames should hold the total number of names in the NAMES array, using the $# special variable. Variable second_name should hold the second name in the NAMES array, using the brackets operator [ ]. Note that the index is zero-based, so if you want to access the second item in the list, its index will be 1.
```
