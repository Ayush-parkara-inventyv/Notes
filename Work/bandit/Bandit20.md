password from[[Bandit19]]
so first i did ssh to bandit20
then i read the hint or the text given on the site
and then tried it 3 to 4 times and understood that the file is sending request to a port which is defined by us and then it is checking the password of bandit20 "current password".

so i thought we already have that password then why dont we just start a listening port and then enter the password of that particular bandit i.e. bandit20 and then tell the file to check the port that we started to listen
then it will check the string with his string and after few attempts i got the password for bandit21
hehehehhehe!!!!!

This is just my thought process dont do like this or misuse the functionality of that file
hehehehehehheheee!!!

steps
ssh to bandit19 and ssh to bandit20 in another tab
then from bandit19 get the password string using the file **bandit20-do**
take that string and then start a listening port
**nc -lvp 4444**
and then enter the password string that you just used 
(!!!!! Dont use command in that listening port i also tried that but it will capture the command as it is and will not execute it)
then it will listen to port

In bandit20 call the file and then give the port on which u started the listening activity
after few tries i got the answer.
**BANDIT19**
![[Pasted image 20250101110820.png]]
**BANDIT20**
![[Pasted image 20250101110922.png]]
password we got there in bandit19: **EeoULMCra2q0dSkYj561DX7s1CpBuOBt**
[[Bandit21]]
