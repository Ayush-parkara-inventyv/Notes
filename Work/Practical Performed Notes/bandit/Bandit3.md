password from [[Bandit2]]
**ssh** to bandit 3
**ssh bandit3@bandit.labs.overthewire.org -p 2220**
now enter password and login
here the hint says that the file is in the directory **inhere**
so check the location of the directory
i got the **directory inhere** i did 
**cd inhere** to redirect to that directory
now there is a hidden file inside this directory and need to check and open it
![[Pasted image 20241218122905.png]]
then i recollected that we can see the hidden file using 
**ls -a**
so after that first i thought giving it the read write execute permissions but it was not required using **chmod**
so did **cat ...Hiding-From-You** with the file name and i was able to see it
the password is 
**password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ**
![[Pasted image 20241218123338.png]]