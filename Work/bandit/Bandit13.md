ssh to bandit13
**ssh bandit13@bandit.labs.overthewire.org -p 2220**
location is given for the stored password
**/etc/bandit_pass/bandit14**
it is visible by bandit14 only
in this we will not get the password but a private ssh key to use it to login into the bandit14
so in the directory itself **sshkey.private** is there
so the only thing we need to do is to use this private key to do ssh login to bandit14 which is the user or say owner of this private key
![[Pasted image 20241218181931.png]]
so we will use **ssh** for the connection and get access to **ssh to bandit14**
![[Pasted image 20241218181839.png]]
command: **ssh -i ./sshkey.private bandit14@localhost -p 2220**
this is bandit14
![[Pasted image 20241218181900.png]]
got the security or private key for ssh
