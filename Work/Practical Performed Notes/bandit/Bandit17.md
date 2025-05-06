as we have already logged in to the bandit17 
To chaliye shuru karte hai!!!!
so as the hint says we just have to find the difference between the old and new passwords file and the new one will be the password
so as we have hint for commands and has **diff** in it so i directly jumped to diff and ran the commands for proper outputs
**diff passwords.old passwords.new**
**diff --normal passwords.old passwords.new**
**diff -u passwords.old passwords.new**
**diff -c passwords.old passwords.new**
the outputs are below
![[Pasted image 20241231160145.png]]
still lets keep both the strings for record
old string:**ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB**
new string:**x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO**
and now lets try to ssh to bandit 18 **ssh bandit18@bandit.labs.overthewire.org -p 2220**
whhattttt!!!!!
twist!!!!!!!!
![[Pasted image 20241231160634.png]]
so i tried the login from another terminal and i got the **byebye** thing so ya things are working fine and the new one is the password
**![[Pasted image 20241231160839.png]]
**