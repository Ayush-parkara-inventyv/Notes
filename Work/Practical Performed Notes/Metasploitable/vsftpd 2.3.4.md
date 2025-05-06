So after login first thing we did is nmap scanning and that gave us the first thing running on the linux which was **vsftpd** on port **21**
so lets check its version and other details 
![[Pasted image 20250103150732.png]]
so as we can see the version and now lets search for any vulnerabilities for the same in **searchsploit** and **metasploit**
![[Pasted image 20250103150843.png]]
and also this said that it is available in the metasploit
![[Pasted image 20250103150954.png]]
from this we can understand that it is a backdoor and we can do remote code execution on it so lets use this vulnerability and get root access to the metasploitable machine.
Commands:
**use 0
show options
set RHOSTS 192.168.177.128
exploit**
![[Pasted image 20250103151218.png]]
and here we have the root access to the machine using nsftpd backdoor.
