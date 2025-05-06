## Introduction
Crack The Hash is a ctf which is based on cryptography and it also gives us basic experience of different types of hashing algorithms that are used now a days to encrypt data for privacy and security concerns.

So it is a good ctf for the beginners to get intro to cryptography and also various ways to instructions and also gives a wide way to approach the hashes for cracking them.

Lets start the CTF without wasting any time.

Before directly jumping to solving and cracking the hashes i want to give you a brief about various ways of doing the same decrypting and encrypting the hashes using various tools as well as free sites that are good enough to use instead of tools which are based on kali.

Tools : 
	[[Hashcat]]
	[[JohnTheRipper]]
	[[RainbowCrack]]
	[[Hydra]]
	[[Medusa]]

 Sites that can be used as an alternative:
	 [Cyberchef](https://gchq.github.io/CyberChef/)
	 [Dcode](https://www.dcode.fr/en)
### Task 1 
### Level 1
 **48bb6e862e54f2a795ffc4e541caed4d**
 this is the string we got here so you can do it using cyber chef also which gives an option to check the hash
 type.
  
Right now i will be using is hashcat and will guide you for the same in this walkthrough...

So first of all we will do is is start hashcat and obtain its type:
```
echo "48bb6e862e54f2a795ffc4e541caed4d" | hash-identifier
```
Running this will give you the hash type....
from this we can see it is md5 now u can use chatgpt or check the codes for md5 in hashcat as we have to give the type of hash for decrypting the hash.

```
hashcat --help | less 
```
 This command helps you to get the number for the hash list (gives whole list)
 ![](https://i.imgur.com/w0OiyFs.png)
```
hashcat --help | grep -i ....#here enter the hash method u want to find code of
hashcat --help | grep -i md5
hashcat --help | grep -i sha256
```
this way you can find all sort of hash code for the hashcat 
![](https://i.imgur.com/MS6BMJV.png)
Now solving the actual hash 

BACK TO CTF😄

**Hash-Type**: *md5*
Now as we already know its type lets crack it with **rockyou.txt**
you can also use fasttrack.txt which is located in the same directory in linux
![](https://i.imgur.com/JV3cCtd.png)

```
hashcat -m 0 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/rockyou.txt --force
```
entering this command will crack the hash and store it in the **cracked.txt** file
also try saving the hash in a file and then put it as an input to the command.
![](https://i.imgur.com/ZvXHz9T.png)

this is the cracked hash stored in cracked.txt file 
![](https://i.imgur.com/tFn9UcT.png)
### Level 2
 **CBFDAC6008F9CAB4083784CBD1874F76618D2A97**
 This is the second one and from now onwards i wont be doing the **hash-identifier** command u can do it yourself i will directly jumping to decrypting the hash.

**Hash-Type**: *SHA-1*

Now lets do the decryption procedure which can be done using this command:
```
hashcat -m 100 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/fasttrack.txt --force
```

We will get the hash as below.....
![](https://i.imgur.com/nAtgX0w.png)

![](https://i.imgur.com/NdMgiRc.png)
this is the raw form for the second string.

### Level 3
 **1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032**
 this is the string for the 3rd one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*SHA-256*

Next decryption of the hash using hashcat:
```
hashcat -m 1400 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/fasttrack.txt --force
```

and the answer we get is shown below:
![](https://i.imgur.com/DYmq2Ek.png)

![](https://i.imgur.com/o7L8Rpb.png)
this is the raw string obtained.
### Level 4
**$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom**
this is the string for the 4th one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*bcrypt*

Next decryption of the hash using hashcat:
for this level it is a bit tricky also you need to use some logic to get the answer faster and use less resources

But if you are very rich and have quantum computers you can surely do the following command directly with rockyou.txt cause in normal pc's with i5 it will take hours to complete and i am happy with this much only so i will be using some basic logic 😜 ; ).

I am saying this because it is very tough to crack for bcrypt so we will be using the rockyou file and as the thm shows no of characters in the answer box so we will be using that as a hint and we will extract all the strings that are of 4 character size and then we will use that wordlist.

Steps for that are as follows:
```
grep -x '.\{4\}' /usr/share/wordlists/rockyou.txt > rockyou_4char.txt
```

this command will give us a file that is of 4 charater strings and we will use this file as wordlist for this level which makes it a bit easy as whole rockyou.txt will takes years and years....

Now decryption to raw form using the below command:
```
 hashcat -m 3200 -a 0 -o cracked.txt hash.txt rockyou_4char.txt --force
```
and the answer is in front of us after some time....
Try not to lose your patience and stop it !!!!!!

![](https://i.imgur.com/hiA0ZD3.png)
this way you can check the status by typing **s** for status ![](https://i.imgur.com/5x4nzYx.png)
and the raw string answer is below![](https://i.imgur.com/wuKkBFR.png)
### Level 5
 **279412f945939ba78ce0758d3fd83daa**
 this is the string for the 5th one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*MD4*

Next decryption of the hash using hashcat:
```
hashcat -m 900 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/fasttrack.txt --force
```

and the answer we get is shown below:
![](https://i.imgur.com/7O0iSP8.png)


### Task 2:
### Level 1

 **Hash: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85**
 this is the string for the 1st one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*SHA-256*

Next decryption of the hash using hashcat:
```
hashcat -m 1400 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/rockyou.txt --force
```

and the answer for the same is given below:
![](https://i.imgur.com/r6Xh3sM.png)

### Level 2
 **Hash: 1DFECA0C002AE40B8619ECF94819CC1B**
 this is the string for the 2nd one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*NTLM*

Next decryption of the hash using hashcat:
```
hashcat -m 1000 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/rockyou.txt --force
```

and the answer for the same is given below:
![](https://i.imgur.com/84TFbPu.png)
### Level 3
  **Hash: `$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.`**
   
   Salt: aReallyHardSalt
 this is the string for the 3rd one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*SHA512*

Next decryption of the hash using hashcat:
```
hashcat -m 1800 -a 0 -o cracked.txt hash.txt /home/kali/Downloads/six_digit_passwords.txt --force
```

using a custom made 6 digit password list.

and the answer for the same is given below:
![](https://i.imgur.com/MnEKPdc.png)

### Level 4
   **Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6**
   Salt: tryhackme
 this is the string for the 4th one.

Lets start the same first finding the hash type and then cracking it.
**Hash-type**:*HMAC-SHA1*

Next decryption of the hash using hashcat:
```
hashcat -m 160 -a 0 -o cracked.txt hash.txt /usr/share/wordlists/rockyou.txt --force
```
and the format to write the hash in the file is as follows:
![](https://i.imgur.com/v1b4q1N.png)

and the answer for the same is given below:
![](https://i.imgur.com/LEQkZKh.png)
DONE!!!

Here we have completed a beginner level cryptography ctf which was nice and fun as well as can teach you the basics for all the various methods used here.