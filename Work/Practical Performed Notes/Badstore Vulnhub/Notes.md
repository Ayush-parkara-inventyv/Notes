The following is a walkthrough of [this vulnhub](https://archive.is/o/g7I17/https://www.vulnhub.com/entry/badstore-123,41/) machine from 2004. I know… it’s crazy old stuff. If this machine would still exist it’d probably look like this:

![](https://archive.is/g7I17/98228c010ee9894a676ea251f09b3961018836b0.webp)

But, for the propose of experimenting with classic low-hanging web app vulnerabilities, it’s still a reliable source for beginners like me. So, the goal was to search for and exploit every vulnerability regarding the web page. My findings include:

- Exposed directories
- Design flaws in password reset
- SQLi
- XSS
- CSRF
- Parameter manipulation vulnerabilities
- Poor hash algorithm
- Poor password policy
- Information disclosure
- Flaws in access privileges for directories and functionalities
- Some default credentials

Let’s start with this!

# 1- Doing reconnaissance and enumeration

First I boot the machine and run an Nmap scan to find where it is:

![](https://archive.is/g7I17/767af9bff228a464aa654523856985c1d2b10994.webp)

Here we can find our target on 102.168.1.5:

Nmap scan report for 192.168.1.5  
Host is up (0.00021s latency).  
MAC Address: 08:00:27:FF:EB:3E (Oracle VirtualBox virtual NIC)

Then I ran a couple more scans to find open ports and service versions exposed:

![](https://archive.is/g7I17/3d827bc3be5388409aa2f96ee7417002665efb39.webp)

![](https://archive.is/g7I17/dbcf5620c9d1ed7bc1bb76cb8fc0675b06751e01.webp)

For now I know the target runs a Linux based OS, Apache 1.3.28 (on ports 80 and 443 TCP) and a MySql server on port 3306 TCP.

Then I go to the site to get a glance of it and what type of functionalities it has. I do this while using ZAP proxy to catch all the requests and feed it with the data it needs to run the scans properly:

![](https://archive.is/g7I17/b6f15d988bd475212e6a87657448b4dc1986fa73.webp)

![](https://archive.is/g7I17/555ccc36bcc0365c1c2f4f6728b1960dbb94a919.webp)

After playing with the site a little I go and download the site in order to do some code analysis:

![](https://archive.is/g7I17/4a19fe2e618de604cfee1ba271a0865745efe134.webp)

![](https://archive.is/g7I17/9b3ed8d5961c84e90d1ab4d5cb4c6a4f0c8ce430.webp)

# 2- Searching for vulnerabilities

In order to find vulnerabilities, I used the information gathered during the previous phase: OS type and version, Services information, open ports, technologies used to build the site, enumeration of inputs and functionalities.

With all that data I ran automated spidering and vulnerability scans using ZAP with custom configs, directory brute-force using DirBuster and test for XSS and SQLi both manually and using automated tools. Also, analyze the parameters and structure of every request using the browser debugger while surfing the site:

![](https://archive.is/g7I17/0a6afef8a305f57518ed503633a518a4c88940fc.webp)

DirBuster brute-force for action inside badstore.cgi

![](https://archive.is/g7I17/46564e1c3965415173421744716057de541d7e89.webp)

DirBuster looking for hidden files or directories using a wordlist

![](https://archive.is/g7I17/e8cc7f223db2d31e9a7835778486b38b3263019c.webp)

sqlmap testing for injection on “cartadd” action

# 3- Findings

Let’s start with a big one! Observing the parameters for the registration request I noticed the role of the user to be added is actually part of the payload. This allowed me to create new admin users by modifying the parameter:

![](https://archive.is/g7I17/7b9e539250d3665fc1e9ce7b54bf7a405d285512.webp)

Next! There is a publicly accessible directory containing sensitive information on /supplier:

![](https://archive.is/g7I17/069c8e5027a239fd4d2f48b9c7760bcfab2f48d3.webp)

Since the data on /supplier/accounts looked like a psswd file from Linux I figure out after some time that the data was base64 encoded:

![](https://archive.is/g7I17/c43aee5ca196196873c1ee58d3d67cafc15e2eeb.webp)

There we have… plain text credentials!

Found another exposed directory on /backup containing even more sensitive data: This time it’s a couple of backup tables of orders and users… so bad :(

![](https://archive.is/g7I17/eb8ac812040a12133ead5d8484416944e9f7b0f1.webp)

To make this even worst passwords are in MD5 and they are not strong passwords at all. As an example I used heinrich@supplier’s password:

heinrich@supplier.de / 5f4dcc3b5aa765d61d8327deb882cf99

Using hash identifier I confirm that it’s MD5

![](https://archive.is/g7I17/e879fb99edc2184a2223df85b48593b6cb81aafc.webp)

Then I can simply use an online service to recover the plaintext:

![](https://archive.is/g7I17/9f098bb52e91c57b4b294df081791a774853fc61.webp)

password password is password

To complete the triad of information disclosure we have a third directory exposed in /DoingBussiness containing a ‘contract’:

![](https://archive.is/g7I17/386eeaa7a151a895fe3b8791e8e5dc80b3786c0a.webp)

![](https://archive.is/g7I17/966a1e44f201c9289c3b9ca11effdc9f8fcd1246.webp)

Much lorem such ipsum

Actually, I found a fourth accessible directory but… given the circumstances of the previous findings this is nothing:

![](https://archive.is/g7I17/2018714ff604546a4dd096c746178f9b559aaaaf.webp)

I shouldn’t be able to access this directory like this and I should be able to see the UploadProc.html file only if I’m a supplier. But given the severity of the other vulnerabilities found and the fact that the file does not reveal information that is not already public this is not a big deal… I won’t be that hard on poor Badstore.

Now… this is huge… there are a couple of severe design issues with the password reset functionality:

- You can reset a password just by supplying a username. The password hint is not even been used.
- The password hint is predefined between 3 options
- The functionality always resets the password to the predefined, hardcoded string ‘Welcome’

This results in the following issues:

- Even tough guessing the password hint would be trivial since there are only three possibilities it’s not even necessary because the site does not check for it.
- We only need a valid username to reset its password (and if we don’t have any we could brute force that to death because the site does not have a restriction for that)
- We could try the password ‘Welcome’ on any user we find since there’s a chance it’s a valid password

Taking advantage of this issues I reset the admin password and get access to the admin panels:

![](https://archive.is/g7I17/3a7f593691c99f9e8ad373c3aa6afd425f36a860.webp)

![](https://archive.is/g7I17/d84896f83b338d06e040dfb182ab19ab0111323a.webp)

![](https://archive.is/g7I17/c2a25b4c232187f8538010c6570e044e5d1043d5.webp)

![](https://archive.is/g7I17/b034f513ac1920c2e98164cc9ab82862f5eb2534.webp)

![](https://archive.is/g7I17/d5960e7617023b922b30809e6152691fd68417db.webp)

Fuckign Kevin Spander doesn’t even have a password! C’mon men!

Also, besides this, the main page for the admin panel is accessible even for unregistered users:

![](https://archive.is/g7I17/47746d692dc68bba6da8f4942b558e16460aebdb.webp)

This gives critical information about how the menu is and which type of operations can an admin perform using it with details about how are the request formed. The operation can’t be performed unless you are an admin but a lot of useful data is revealed.

Another huge problem here is that the traffic is not encrypted at all. Anyone sniffing the traffic could obtain plain text credentials as easily as this:

![](https://archive.is/g7I17/7e992764d29e1226428b8c91e633e1ccf4a58de7.webp)

As we’d expect from a bad store, XSS is not missing. I managed to take over browsers using Beef by exploiting an XSS vulnerability in the guestbook:

![](https://archive.is/g7I17/8265ec959e47177770890fc6480f8eb4563fc84e.webp)

![](https://archive.is/g7I17/628816234409df28e8f8c4b56b1a9f47ee0522ed.webp)

Beefy!

The login is also broken. A simple SQLi query can give us access without having to provide a username. The SQLi becomes really obvious after trying using a quotation mark and obtaining the following error message:

![](https://archive.is/g7I17/9333557b6482e0b4aea05fedef617faeb77e9645.webp)

Using a string as _admin’ or ‘asd’=’asd_ as the email address let us log in as an admin user:

![](https://archive.is/g7I17/299a118c913013d95cada9f7edbf58af0e5a06a3.webp)

![](https://archive.is/g7I17/98485f702e335a4dd6cd6bb8ee47a4934c488843.webp)

These guys also left a test page there… full of secrets:

![](https://archive.is/g7I17/458ae2123ce575687d4282e8be198e3150f6d4cc.webp)

z3cr37z

![](https://archive.is/g7I17/7d5fd3ea97005ee6608404d94f49b79b5dc973e7.webp)

m00r3 z3kr375

Oh! look! There are backups available too:

![](https://archive.is/g7I17/167e90bf5f003ead3e4f4816cb68710324af3f80.webp)

Last but not least: We go Sea-Surfing

The store has no protection against CSRF. We can observe this on user creation. There are no tokens being sent or received during the process of creating a new user:

![](https://archive.is/g7I17/1955c9b14991229bef5e63686f91dc85df5f19e1.webp)

After having collected the data needed to create a new user I now proceed to implement a javascript payload to repeat the request:

![](https://archive.is/g7I17/5bd61b5d8bd67cedca86abbc65d55c5912d242c4.webp)

I serve the payload in my local machine on port TCP 666:

![](https://archive.is/g7I17/f521b16b046279dd1658b73084ea33ab1ea365dc.webp)

If a user would be tricked to go to my malicious link http://192.168.1.4:666 the payload would be executed:

![](https://archive.is/g7I17/ec659b1288097b864ed76ef837831671cdc3356f.webp)

Creating a new user with admin privileges from an IP that’s not even mine, using the resources of a victim:

![](https://archive.is/g7I17/185cfad5ca333b5ff5049918d13fd7843227f1c6.webp)

This could also be used to purchase items in the store and/or abuse any other functionality that the store is offering.

Lastly, we have another possible attack vector brute forcing the DB credentials directly since the service is available for remote access.

The initial Nmap scan performed revealed a MySQL instance running on the default port. Using Metasploit mysql_login scanner module I was able to get the credentials for the database as follows:

![](https://archive.is/g7I17/0c6bab5965e318b1a169241c090ac281b1826804.webp)

root root root root

After having run the module we can go straight to the DB, check for the credentials and start messing around with it:

![](https://archive.is/g7I17/21d8ba6c4e445c8965af1a9c7c8f43d661e3d9d1.webp)

Apart from all the vulnerabilities I mentioned I also noticed there are no protections set on the headers (like X-XSS-Protection, Content-Security-Policy, and X-Frame-Options) and also Apache, MySql and OpenSSL are extremely outdated but since this Vulnhub image is from 2004 I don’t think those are things to be considered right now.

So… that’s all. I hope you like this writeup and see you around!