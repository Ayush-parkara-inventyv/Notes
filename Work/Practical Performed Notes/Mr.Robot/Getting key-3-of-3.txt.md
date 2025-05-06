Now as we got the access to robot user as well as the 2nd key
The only thing left is the last key which will be accessible by the root user only as it is situated in the **root** directory.

For that after getting into robot user we can exploit or can get priviledge escalation by 2 of these methods which can be understood by the permissions given to the robot user.
The two methods are as follows:
**suid binaries or cron tabs**
So i am using suid binaries to open shell 
which can be done using the the nmap which can be detected by the permissions for directories that nmap can be exploited
so commands for the following are as follows:
**/usr/local/bin/nmap --interactive**
and see it works it is interactive and means it is vulnerable 
so now enter the command **!sh**
and hoorah
we got the root access and successfully did the priviledge escalation.
![[key3_photo1.png]]
Okay so the last sweet step of getting to the key and reading it
**cat /root/key-3-of-3.txt**
![[key3_photo2.png]]
key-3-of-3.txt:**04787ddef27c3dee1ee161b21670b4e4**
and here we have completed the Mr. Robot CTF.

and one awesome thing that i want to share to you is that you can do these steps and get the root access directly from the daemon only no need to do the changing of user to robot and then switching to root.
Reason is that nmap which is installed on the system is accessible from the daemon also and this version of nmap is vulnerable and can prompt root access shell to the attacker.

I have checked and confirmed it hehehehehee!!!!
![[key3_photo3.png]]
So this is it.
