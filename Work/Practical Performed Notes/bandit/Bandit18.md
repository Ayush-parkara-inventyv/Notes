As we knew previously that we are not allowed to connect to bandit18 because of the bashrc file which is logging us out when  tried to login 
so the only thing we need to do is to ssh bandit18 but with no profile so we only have to add a string after the normal ssh command **"bash --norc --noprofile"**
so i will run the command 
**ssh bandit18@bandit.labs.overthewire.org -p 2220 "bash --norc --noprofile"**
![[Pasted image 20241231164116.png]]
and then enter the password we got last time in bandit17 
and see first i didnt realised but we actually got the ssh to bandit18
and then the readme file with the password for bandit19
sweettt!!!!!!!!
**cat readme**
![[Pasted image 20241231164309.png]]
password: **cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8**
dont worry this is the right password i have already checked logging in to the bandit19 using this
hehehehehe!!!!!
