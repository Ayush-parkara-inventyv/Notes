ssh to bandit15 **ssh bandit15@bandit.labs.overthewire.org -p 2220**
password from the previous one
now it is said that use ssl/tls to send password to port 30000
so i thing we will be using ssh as it uses ssl/tls connection
so lets check the password first the password used for login is same with the actual password or not 
visiting the same directory to view password for bandit15
first i did ls and got the bandit14 password by **ls**
![[Pasted image 20241231132650.png]]
so the thing i can understand is that password is same and there is no bin/bash directory to check the password of bandit15
but the methodology to sent the password is by using a tool that uses ssl and tls certificates.
so first i tried by ssh but it was complete fail
Then i used openssl s_client to connect and i got some hint as the port was wrong
![[Pasted image 20241231140015.png]]
so the command was almost right so i did some changes and redirected it to right port no.

From that i noticed one thing that the password is right but because of self-signed certificate i am not able to do the connection.
![[Pasted image 20241231140212.png]]

so now i had to do is find a way to bypass this certificate and use no certificate for connection.
few commands i tried
**echo "rudy:8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -noverify**
here noverify doesnt exist.

**echo "rudy:8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -CAfile /dev/null**
as u can say i tried dev/null certificate but it was of no use.
![[Pasted image 20241231140452.png]]

then i used **-ign_eof** and as i was using rudy also as input i got an error of incorrect as u can see in below photo.
so i removed the rudy from the string getting sent and then i got the right answer.

command to do ssl/tls connection: **echo "8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo" | openssl s_client -connect localhost:30001 -ign_eof**
![[Pasted image 20241231135626.png]]
the password is correct
![[Pasted image 20241231135648.png]]
the string we got from the output: **kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx**
