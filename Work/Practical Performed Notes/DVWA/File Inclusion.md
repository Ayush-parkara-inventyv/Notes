**_What is File Inclusion Attack?_**

It is an attack that allows an attacker to include a file on the web server through a php script. This vulnerability arises when a web application lets the client submit input into files or upload files to the server. A file include vulnerability is distinct from a generic Directory Traversal Attack, in that directory traversal is a way of gaining unauthorized file system access, and a file inclusion vulnerability subverts how an application loads code for execution. Successful exploitation of a file include vulnerability will result in remote code execution on the web server that runs the affected web application.

**_This can lead to the following attacks:_**

1. Code execution on the web server
2. Cross Site Scripting Attacks (XSS)
3. Denial of service (DOS)
4. Data Manipulation Attacks

**_Two Types:_**

a) Local File Inclusion

b) Remote File Inclusion

LFI vulnerabilities allow an attacker to read (and sometimes execute) files on the victim machine. This can be very dangerous because if the web server is misconfigured and running with high privileges, the attacker may gain access to sensitive information. If the attacker is able to place code on the web server through other means, then they may be able to execute arbitrary commands.

RFI vulnerabilities are easier to exploit but less common. Instead of accessing a file on the local machine, the attacker is able to execute code hosted on their own machine.

Remote File inclusion (RFI) and Local File Inclusion (LFI) are vulnerabilities that are often found in poorly-written web applications. These vulnerabilities occur when a web application allows the user to submit input into files or upload files to the server. In order to demonstrate these attacks, we will be using the [Damn Vulnerable Web Application](http://www.dvwa.co.uk/) (DVWA).

**_there are some pre-requisites required:_**

1. XAMPP
2. [Damn Vulnerable Web Application](http://www.dvwa.co.uk/) (DVWA)

_NOTE_: Currently, lets focus on file inclusion attacks. I am going to show you how to setup lab using xampp, dvwa and many more in my next upcoming blogs.

**_Local File Inclusion in Action_**

Since you have an idea what LFI is, let’s see it in action. We will perform LFI attacks through different levels of difficulty offered by DVWA.

Let’s start with low difficulty.

**_Difficulty: LOW_**

Now start your machine and login to DVWA, then go to DVWA security tab and change the difficulty level to low.

**![](https://i.imgur.com/goxlNIz.png)


Go to file inclusion tab and change the URL from **incude.php** to **?page=../../../../../../etc/passwd.**

![](https://i.imgur.com/23rTP5J.png)

![](https://i.imgur.com/GBYXYQg.png)

change the URL from**?page=../../../../../../etc/passwd** to **?page=../../../../../../proc/version**.

![](https://i.imgur.com/g71zWxp.png)

**_Difficulty: MEDIUM_**

Now, go on and try the exploits we used in low difficulty. You will notice that you can’t read files like before using the directory traversal method. So, as you can see in the below snapshot of source page, the server is more secure and is filtering the ‘../’ or ‘..\’pattern. Let’s try to access the file without ‘../’ or ‘..\’.

![](https://i.imgur.com/DPWBtOn.png)

Change **include.php** to **/etc/passwd**

![](https://i.imgur.com/lCZjSjw.png)

Now,change the URL from**?page=/etc/passwd** to **?page=/proc/version**.

![](https://i.imgur.com/v8QTOGC.png)

As you can see, it worked by directly entering the name of the file. Let’s level up the difficulty to HIGH.

**_Difficulty: HIGH_**

Change the difficulty to HIGH and try all exploits from medium difficulty, and you’ll notice none of them will works because the target is more secure, as it is only accepting “include.php” or inputs starting with the word “file”. If you try anything else, it will show “File not Found”.

![](https://i.imgur.com/3zjcr1c.png)

In this level of security, we can still gather sensitive info using the “File” URI scheme. (because it starts with the word “file”)

Change the URL from **include.php** to 
```
?page=file:///etc/passwd
```

![](https://i.imgur.com/BIfMS48.png)

You will get the data of **/etc/passwd** file.

This is how you can exploit file inclusion vulnerability using local files on the webserver.


![](https://miro.medium.com/v2/resize:fit:600/0*bv2zv9QXEGdkdhUR.gif)

**_Remote File Inclusion in Action_**

Now, let’s try to exploit this vulnerability using remote files hosted on the attacker machine.

**_Difficulty: LOW_**

Now, Let’s start with the Low difficulty.

Change the difficulty to low and go to file inclusion tab.

Let’s change **include.php** to [**http://www.google.com**](http://www.google.com/) so the final URL will look something like this,

**?page=http://www.google.com**

![](https://i.imgur.com/0iJZUef.png)

**_Difficulty: MEDIUM_**

Change the difficulty to medium and check as we did it in the low difficulty. You’ll notice, it’s not working anymore. The target is now filtering “http” and “https” as shown in source page.

![](https://i.imgur.com/QFvlvG4.png)

so try the attack with “HTTP” (in CAPS) or any one word in caps like I used as shown in snapshot (httP)and it’ll work.

```
?page=httP://imdb.com
```
![](https://i.imgur.com/6dK6WDD.png)

**_Difficulty: HIGH_**

![](https://i.imgur.com/wFOewM1.png)

We can’t exploit the high difficulty using RFI as you can see in source page,we know that the target web-server is only accepting “include.php” or anything that’s starting with the word “file” that’s why we can’t include anything from an outside server.

**_Points to Secure against File Inclusion Vulnerability_**

a) Strong Input Validation.

b) A whitelist of acceptable inputs.

c) Reject any inputs that do not strictly conform to specifications.

d) For filenames, use stringent whitelist that limits the character set to be used.

e) Exclude directory separators such as “/”.

f) Use a whitelist of allowable file extensions.

g) Environment Hardening.

h) Develop and run your code in the most recent versions of PHP available.

i) Configure your PHP applications so that it does not use register_globals.

j) Run your code using the lowest privileges.

**_That’s how you exploit and secure against file inclusion vulnerability. There are many other ways to exploit file inclusion, other than which I mentioned in this post. The exploits I mentioned here are easy and can be easily performed. If you find some other cool ways to exploit file inclusion, do share them in comments, I would love to improve myself._**
