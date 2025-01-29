ssh to bandit14 done already
now the hint is given that we need to send the password of this stage to the next level on **port 30000** on **localhost**
I did **ls -a** and got a **.ssh** directory
**cd ./.ssh**
![[Pasted image 20241218182336.png]]
then i found the rsa public key for next one where rudy@localhost is the user
then tried the same command like previous and got the following output![[Pasted image 20241218182844.png]]
So i thought lets check the /etc directory to check the passwd
so i did **cd /etc**
then i checked the list and then checked the contents of the file **passwd**
![[Pasted image 20241231123119.png]]
so now i visited the **/bin/bash** location and there i found many file and then i got attracted by this **bandit_pass**
i checked the contents of it and got the password of bandit14
![[Pasted image 20241231123405.png]]
password of **bandit14**: **MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS**
![[Pasted image 20241231123937.png]]
i tried various commands to send the user and password to the localhost 30000 but it showed the password is wrong for user rudy.
But but but i have not checked using bandit14 so lets check that first then i will think of other thing.
I was doing the  wrong thing the whole time why need rudy 
it is said to send password to localhost 30000 thats it.
Command: **nc localhost 30000**
![[Pasted image 20241231131714.png]]
The string i got at the end which can be used for 15 i think
String : **8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo**
yes use this string to login in to bandit15