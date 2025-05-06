first do ssh to bandit32 using password from [[Bandit31]]
**ssh bandit32@bandit.labs.overthewire.org -p 2220**
then we got ssh uppercase shell
the only command works is 
**ctrl+z** ,**&** ,**/...(any directory)**
hey from these things i got the command for reexecuting the shell 
**$0**
so lets try that
![[Pasted image 20250107181512.png]]
so then lets **cat** to bandit33 password
**cat /etc/passwd | grep bandit33**
then it gave /bin/bash
so **cd /bin/bash**
then we have a readme file and then this text comes when we do **cat**
![[Pasted image 20250107181637.png]]
BINGO!!!!! bandits are completed