use password from bandit3
now **ssh** in bandit4
**ssh bandit4@bandit.labs.overthewire.org -p 2220**
after login 
the hint says the file is human readable in **inhere** directory
first i did **clear** to clear the terminal
i tried l**s -a**
**ls**
**ls --help** to check whether there is any file showing notation or way to do so
Also tried to do cat with the diff files like **cat "-file00"** and **cat file00** and **cat 00**
no proper outcomes were there
![[Pasted image 20241218125614.png]]
tried different ways to see the human readable file also
![[Pasted image 20241218130052.png]]
finally after realizing the silly mistake i was making 
did **cat ./** for all file names 
i got the password inside the **file07**
**cat ./-file07**
**password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw**
