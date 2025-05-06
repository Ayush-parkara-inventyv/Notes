ssh to bandit9
**ssh bandit9@bandit.labs.overthewire.org -p 2220**

now as the hint says it is human readable and also it is followed by several = characters.

so first of all as it is human readable form and whole file is binary or pure data
![[Pasted image 20241218155256.png]]
here we can see by **grep** it sas binary file
so thing we can do is first of all check the human readable things can be captured by **strings**
so we can use strings and get all the ascii charater format thing
in that we can apply the filter or say **grep** by giving the specifier **=** so the command we use is this as it says several we can use more than one = sign
**strings data.txt | grep "= ="**
then we will get the output
![[Pasted image 20241218155608.png]]
password: **FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey**
