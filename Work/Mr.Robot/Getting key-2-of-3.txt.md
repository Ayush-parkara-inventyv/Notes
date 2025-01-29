As we found a directory in the **robots.txt** now we will check that dictionary file and see what do we have there.
the name is **fsocity.dic** dont type it directly just like me and type fsociety.dic!!!!!!.

tihs is the output that i got from the .dic file
![[key2_photo1.png]]
this is a long dic and now we will do copy this to our local machine from any way u want to.

The only thing i will do is copy this and make a new file and then paste it that's it.
![[key2_photo2.png]]
then u can check the file content in terminal and then you will notice that the files contains so many duplicates 
so i did is first sort the file and then save the unique strings in a new file.
which is done by the command **sort** and **grep** and **uniq**.
**grep -oE '[a-zA-Z]+' fsociety.txt | sort | uniq > f.txt**
so we have a sorted file.

Now time to check other directories from the gobuster output we got.
so you can check each i will be directly moving to the **/wp-admin**.
Sorry for the change in ip as i faced some connection issues you can continue as per you ip.
it looked something like this:
![[key2_photo3.png]]
now as u can see we r asked to enter username and password and for this site u can check one unique thing in which if the username is incorrect it will show invalid username which will be easy for us to get the true username through bruteforce.
![[key2_photo4.png]]
so lets do the brute forcing using the dictionary file we got from the server.
We can use various tools like **hydra** , **burpsuite** as well as **wfuzz**.
i will use **hydra** as well as **wfuzz**.
**hydra -L f.txt -p test 10.10.164.176 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:F=Invalid username"**
so this shows that we will use f.txt file for bruteforce of username then set password with some random text instead of blank.
also set the fail string as **Invalid Username**
which will be easy to set.
the output we get is this
![[key2_photo5.png]]
so here we got the username as we got the error which is not equal to incorrect username so we will use these  usernames and then again use the same file for password brute force and get the correct credentials.
Usernames: **ELLIOT, Elliot**
![[key2_photo6.png]]
now bruterforce for password with same password file 
now using **Wfuzz** for the bruteforce of password 
the commmand for that is 
**wfuzz -c -z file,f.txt -d "log=ELLIOT&pwd=FUZZ" http://10.10.117.182/wp-login.php**
![[key2_photo7.png]]
the password we got
![[key2_photo8.png]]
password:**ER28-0652**
![[key2_photo9.png]]
Here we got the login
now u can check and browse and see the services we can use and exploit.
i'll move start to the further step i.e. getting a reverse shell.
so we will use the **pentestmonkey** and generate a shell script.
Also while it is getting generated u can also check the place in which we are going to inject the php script code and then execute it on the action performed.
Link for pentestmonkey: **https://github.com/pentestmonkey/php-reverse-shell.git**
![[key2_photo10.png]]
Location for the shell script injection: Inside of ELLIOT account
**Appearances -> Editor -> 404 Template**
![[key2_photo11.png]]
This is the reverse shell script that is already made and u have to use it directly
**IMPORTANT POINT change the credentials that are highlighted in the image.**
so i have changed the ip and port no is 4444 for listening on your attack machine.
Now we will do 2 steps first copy the complete script and then paste it below the code of 404 template and then update it
second open a terminal and start listening on port no 4444
![[key2_photo12.png]]
check the photo and then click update
now open terminal and run this code to start listening on port 4444
**nc -lvnp 4444**
now this means that you have to enter a uri or a page which does not exist on the elliot account. which will call the 404 template and our code will get executed and we will get reverse shell.
**http://10.10.117.182/wp-admin/page=45**
![[key2_photo13.png]]
by hitting this url you will be prompted to your shell which will be waiting with the reverse shell you just obtained.

Now obtain a shell and then check the files that are there in robot directory
the commands we used for getting here.
**bash -i**
**whoami**
**cd /home/robot**
here you will see 2 files first check the password file which contains hash value of password of user robot which is done by md5.
and also when you check the key file you will see that the permission is denied to see that file.
![[key2_photo14.png]]
so now use the password and crack it using john the ripper.
first store the hash file on your target machine and then use this command to crack the hash
the wordlist we are using is rockyou.txt and the hash file name is password.hash in my attack machine.
**john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt password.hash**
then the output i get is this:
![[key2_photo15.png]]
the password is **abcdefghijklmnopqrstuvwxyz**
So now we need to change the user as the daemon wont have much of permissions 
command to change user to robot
**su robot**
then enter password that is decoded above
but it will show the prompt that you need terminal to do that we can use python script which will help us to spawn bash or can say tty terminal 
which will help us to get the second key 2 by accessing the robot user
Command for the same is as follows:
**python -c 'import pty; pty.spawn("/bin/bash")'**
which means that we are prompting the bash with python script or command
and this results like below
![[key2_photo16.png]]
see now we can get the access to robot user 
now enter the password and the second key is yours
output of the same
![[key2_photo17.png]]
key-2-of-3.txt:**822c73956184f694993bede3eb39f959**


