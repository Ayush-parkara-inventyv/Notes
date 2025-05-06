Please create a seperate MD file for all the tasks, as 
task1.md
task2.md and so on

Task 1: Perform NMap scan on 
- google.com
- metasploitable2 machine
- any one random room from tryhackme (any)
  
  
Nmap on google.com
first of all we have to check the ip of google.com 
so what i did is ping google.com by which i got the ip address of google.com and then using that we can do the nmap scan of google.com
<!--⚠️Imgur upload failed, check dev console-->

	![[Pasted image 20250506123733.png]]

<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506123752.png]]

Nmap scan on Metasploitable 2 machine 

I have metasploitable 2 machine in my vm and is on and running 
<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506123950.png]]
now as kali the attacker machine and the metasploitable 2 are both on the same vm we can ping and scan each other as both are on same network

and also i got the ip address to make sure they are connected and visible to each other

<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506124133.png]]
this shows that the machine is up and running and now the nmap scan
<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506124237.png]]

Nmap Scan on tryhackme machine 
for that first we have to connect to the thm server and that is done by using the openvpn and use ovpn file from the tryhackme to connect and get access of the machine 
<!--⚠️Imgur upload failed, check dev console-->

![[Pasted image 20250506124420.png]]then check the ip from the site and then nmap scan
<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506124516.png]]
10.10.80.146 this is the ip and the nmap scan is below
<!--⚠️Imgur upload failed, check dev console-->
![[Pasted image 20250506124535.png]]