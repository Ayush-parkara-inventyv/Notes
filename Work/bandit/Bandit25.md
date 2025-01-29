ssh to bandit25 using password from [[Bandit24]]
okay so as the hint suggests normal ssh wont let you do the login into the bandit26 and just because of that as one can see that in the below ss that i am gettting logged out or say connection closed to the bandit26
first lets check the local directory to get something
**ls**
![[Pasted image 20250107123902.png]]
and here we have the ssh key to the bandit26
good lets try to do login
**ssh -i ./bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220**
![[Pasted image 20250107123256.png]]
as you can see we are not able to login to bandit26 and its not the key or command problem but its the trick that we are going to reveal now
It is so because 
THIS LAB
YES THIS LAB IS TOTALLY OUT OF THE BOX THINKING THING
so if you actually want to give it a shot then go and give it a show its of out of the box thinking the process of ssh is same nothing unique.
and if you get something that is looking buggy or some glitch type or get whole prompt copied then think that you have solved the trick half 
okay so lets move further
first lets check the bandit passwd 
**cat /etc/passwd/ | grep bandit26**
and this is the output we got 
![[Pasted image 20250107124150.png]]
so lets check the new file we got here and inspect what is its meaning
![[Pasted image 20250107124227.png]]
as you can see here the code suggests that the user or cmd should be there and bandit uses linux for that then also next line suggests that when there is a situation that needs more to show more no of lines it will call text.txt
so if u understood this than again u can try further by yourself or can just go on like this 
so we have to do ssh in such a way that we get the more line in the terminal and that is done by resizing the terminal in such a way that it will require more option to show next lines and content.
you dont have to do any other command just use the same ssh command using the private key only twist is to resize the terminal size.
example this
![[Pasted image 20250107124631.png]]
i hope you got the idea what i am trying to say
now comes another interesting part
even if u resize it its your choice but suggested way is to continue like this only
now enter **vim** and hit **Enter**
so this will come![[Pasted image 20250107124756.png]]
now we have to see the password first of bandit26
you can also get the shell of bandit26 directly but it is unstable and if u resize it or do any unnecessary moves it may get closed as i got it closed thrice!!
now we have to do is enter the command 
first enter anything using **shift**
then this will be prompted![[Pasted image 20250107125029.png]]
do **esc**
this will be prompt and the string at the bottom is added by me so u can also add the commands and here you have to enter this command
**:set shell=/bin/bash**
![[Pasted image 20250107125334.png]]
i will get the shell of bandit26 using this 
now hit **enter** and again type **:shell**
![[Pasted image 20250107125457.png]]
we got the shell and without wasting any time check the password for bandit26
or you can also check the password for bandit27 directly.
![[Pasted image 20250107121045.png]]
password of bandit26:**s0773xxkk0MXfdqOfPRVr9L3jJBUOgCZ**
Hence you have solved the bandit25->bandit26
and try to get the bandit27 password in the same session otherwise you have to do the same things again even if u have the password.