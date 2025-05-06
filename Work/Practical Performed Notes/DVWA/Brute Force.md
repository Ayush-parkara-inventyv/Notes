This write-up covers the brute force attacks on the low, medium, and high security levels of DVWA (Damn Vulnerable Web Application) targeting an HTTP login page.
![](https://i.imgur.com/FZq4ZQg.png)

Lets start with the **low** security level.
![](https://i.imgur.com/TOFENFD.png)
From here you can set the low, medium and high security level for getting tougher levels.

Inside DVWA, I selected the Brute Force option, which takes me to a Login page.
![](https://i.imgur.com/srNgzHE.png)


So for this you can use various types of user and password lists, in my case i am using the fasttrack.txt  that is already available in the kali linux at the location:
```
/etc/share/wordlists/fasttrack.txt
```
and will be using this only for the brute-forcing using the **intruder** in burp suite.

The **Intruder** feature in Burp Suite automates the initiation of attacks on web applications. Its primary purpose is to systematically dispatch a large volume of requests to a target application, utilizing various payloads to examine and pinpoint vulnerabilities. 

These could encompass input validation challenges such as SQL injection, cross-site scripting (XSS), buffer overflows, and directory traversal exploits.

Here we can **Choose an attack type**, **Add** or **Clear** payload markers, and **Start attack**. I have inserted a new payload marker and you can see that in the password value:
![](https://i.imgur.com/37V38tm.png)

Now next step is to set the payloads and in that we have to enter the list of passwords which is **fasttrack.txt** in our case.

So click on load and load the password list:
![](https://i.imgur.com/hyJMd3K.png)

After doing this you are ready to start an attack and get the password.
For knowledge here default username is admin so it is good to use it directly 
hehehehee!!!!!
```
password=password
```

Now using the credentials got to the login page and attempt login.

Username: **admin**
Password: **password**
![](https://i.imgur.com/hsjO2MI.png)
It worked! The Brute Force attack was successful.

**Medium Level:**
Now, let’s replicate the previous approach. Capture the login request, transmit it to the Intruder, designate the payload injection point, define the payload, and commence the attack.

We will again get the same result and the password is correct too.

In DVWA we are easily able to check the PHP source code for all security levels, just click the view source button at the bottom of the DVWA Window. This will display the source PHP which shows the Login failed Sleep( 2 ) command as pictured below.
![](https://i.imgur.com/pELXEGH.png)

This shows adding a time delay on failed logins is not a deterrent or adequate protection from a brute force attack. The wait delay only increases the time needed to perform the attack, slowing down the attack speed.

and you will be able to solve the medium level using the same methodology.

**High Level:**
This is the source code for high security level:
![](https://i.imgur.com/WR2X0CP.png)
The strong security system uses something called a CSRF token. This token is like a secret code created by the server and given to the user’s device.
![](https://i.imgur.com/0mthaF9.png)

So here you can absolutely do it using the **pitchfork** attack from the burpsuite only..

But But but lets get some change and head over to our sweet little terminal.
The most common username for brute force is **admin** and lets use **fasttrack.txt** as a wordlist as it is small and better than **rockyou.txt**.

so the command using hydra for brute force is as follows:
```
hydra -l admin -P /usr/share/wordlists/fasttrack.txt 127.0.0.1 -s 42001 http-post-form "/vulnerabilities/brute/:username=^USER^&password=^PASS^&Login=Login:F=incorrect"
```

![](https://i.imgur.com/Dum6JJ9.png)

here we got the username and password for the site.
![](https://i.imgur.com/VX7jYgH.png)
and you can seeee how easy that way just 1 command and the password is in front of your eyes.

Thank You!! 
and see you in the next writeup 
Happy Hacking!!!.


