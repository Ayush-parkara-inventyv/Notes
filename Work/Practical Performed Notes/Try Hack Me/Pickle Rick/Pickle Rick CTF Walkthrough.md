## Introduction
The Pickle Rick CTF is an easy challenge that can be completed quickly. To approach this challenge, we can follow a standard methodology for tackling any machine IP address. We'll start by performing an **nmap** scan on the provided IP address.

### Nmap Scan
The **nmap** scan reveals that there is a website running on HTTP port 80.
```bash
nmap -sV -sT 10.10.188.20
```
The output shows that the website is up and running.
![](https://i.imgur.com/hypTmxt.png)
### Visiting the Website
Upon visiting the website, we find that it contains interesting content.
![](https://i.imgur.com/23OJZBh.png)

### Checking the Source Code and Robots.txt
We need to check the source code of the website and the **robots.txt** file.
![](https://i.imgur.com/VdoStsw.png)
The **robots.txt** file reveals a username and a string that could be the password.
![](https://i.imgur.com/0L3j468.jpeg)
The username is **_R1ckRul3s_** and the string is **_Wubbalubbadubdub_**.

### Dirbuster Scan
We'll perform a **dirbuster** scan to find any hidden directories or files.
```bash
sudo dirbuster 10.10.188.20
```
The output shows that we have found a few directories and files, including **assets**, **login**, and **denied**.
![](https://i.imgur.com/fvGbo1T.png)
![](https://i.imgur.com/keqUDZq.png)

### Login Page
We'll try to log in using the username and password we found.
![](https://i.imgur.com/PTrRFi8.png)
After entering the username and password, we gain access to the portal page.
![](https://i.imgur.com/QZUEaxj.png)

### Command Panel
We can run commands and get output from the command panel. We'll try a few commands to see what works and what doesn't.
Few things to note:
- **ls** is working
- **cat** is not working, but **less** is a working alternative
- We don't need **cd**, we can manage with **ls** and **less**

### Finding the First Ingredient
We'll use the **less** command to view the content of the file **_Sup3rS3cretPickl3Ingred.txt_**.
![](https://i.imgur.com/ofPjbg9.png)

### Finding Non-Working Commands
To find the non-working commands, we'll use the **grep -R** command.
![](https://i.imgur.com/0wdtLv5.png)
We'll check the source code to find the list of non-working commands.
![](https://i.imgur.com/Qb0jX7Y.png)

### Sudo Command
We'll try the **sudo -l** command to see if we have any restrictions.
![](https://i.imgur.com/96eymMe.png)
We find that there are no restrictions on using **sudo**, making our task easier.

### Finding the Next Two Flags
We'll use the **sudo ls** command to list all the directories and files on the system.
```bash
sudo ls ../../../*
```
![](https://i.imgur.com/jEGKszG.png)
We'll check the list to find the **home** and **users** directories. The third flag should be in the **root** folder.

### Second Ingredient
We'll use the **less** command to view the content of the file **second ingredients** in the **rick** user directory.
```bash
sudo less /home/rick/second\ ingredients
```
![](https://i.imgur.com/syJ7AfO.png)

### Third Ingredient
We'll use the **less** command to view the content of the file **3rd.txt** in the **root** directory.
```bash
sudo less /root/3rd.txt
```
![](https://i.imgur.com/FOSV4mD.png)

### Conclusion
We have found all the secret ingredients. We work out the potion and finally transform Rick back to a human from a pickle!
![](https://i.imgur.com/MeCvTOs.gif)
See you then in the next post.