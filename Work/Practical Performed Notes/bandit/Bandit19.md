use password and do ssh to bandit19
now as it is said that we have to do suid binary
so i tried **./**
and previous that we ran few commands to check the file permissions 
![[Pasted image 20241231180203.png]]
so it prompted to run this command as another user
so as the hint was already there
**./bandit20-do id**
also i tried **id** and from that i got that this file is executting the commands with root permission 
so i tried executing the cat command for password of bandit20
**./bandit20-do cat /etc/bandit_pass/bandit20**
and hoorraahhhhh!!!!!!
i got the password
![[Pasted image 20241231180439.png]]
password19:**0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO**
