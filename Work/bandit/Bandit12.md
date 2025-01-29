ssh bandit12
**ssh bandit12@bandit.labs.overthewire.org -p 2220**
first it is given hexdump as hint and previous steps were performed as denoted and made file in tmp directory

![[Pasted image 20241218162233.png]]
![[Pasted image 20241218162430.png]]
now it also say it is repeteadly compressed 
so first convert it back from hexdump or hexadecimal to binary using **xxd**
**xxd -r data.txt > binary**

after getting the binary then check the file type or compression type and then convert it into that format directly and then go on decompressing the file until we get the final ascii text of password

![[Pasted image 20241218171908.png]]

tries done 8
**password:FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn**