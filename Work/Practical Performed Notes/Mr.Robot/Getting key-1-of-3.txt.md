So first lets start the machine 
also dont forget to start the **openvpn** to the tryhackme access
i hope u have completed till that 
now we will be prompted with this 
the machine ip i was using is **10.10.24.180**
![[Pasted image 20241227110312.png]]
here we got 6 commands and these 6 only work in the prompt u can check each of them by typing no need to show them here!!!!!.

Lets move further
so first thing is all these clips should be stored somewhere at the server cause we cannot get access to it directly.

so we will do 2 things first check the services using **nmap** and we will also use **gobuster** as we want to see the directories and places we can visit to get further info from the server.

![[Pasted image 20241227110911.png]]

and then as we can see the ssh is closed the only thing we have is tcp port 80.
but without focusing on only this thing lets also check the server for further files and directories.

We will be using **gobuster** for this 
file used for searching is common.txt 
path:**/usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt**
(you can get this list if u dont have one from the github
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/common.txt)

command for gobuster:
**gobuster dir -u http://10.10.24.180:80/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -b 500**
here i have used -b for removing the status code 500
Also with that the most impoortant thing that one should remember to check the robots.txt first in any web application or web page
As it can provide us the list of all the items are hidden or not in the actual directory list so u can get more idea and hints from it.
![[Pasted image 20241227112146.png]]
this is the output i got from the gobuster
here the things we see is that the mr.robot has a robots.txt file also has a account in **wordpress** i.e.
**/wp-admin** and also if u r curious u can check each ones but the things we need is robots.txt and the admin login page for the wordpress.

First check the **robots.txt**
![[Pasted image 20241227112419.png]]
see this thing always helps now u got the first key i.e in the file **key-1-of-3.txt** and and and u also got a dictionary i.e **.dic** extension which will be useful somewhat in future reference.

Contents of File:**key-1-of-3.txt**
**073403c8a58a1f80d943455fb30724b9**
