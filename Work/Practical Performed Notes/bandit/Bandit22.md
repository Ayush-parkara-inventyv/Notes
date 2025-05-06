ssh to bandit22 using the password from [[Bandit21]]
Okay so as the text on the site suggested that we have to see the cron.d directory and see the bandit23 file
so it gave the directory in which the cronjob is located 
then i checked the content of the file and then as the job suggests
set the **myname** as **bandit23**
if u do use it with bandit22 then it will do the same for bandit22
so whoami will be bandit23 u can try yourself
then after that the my target suggests that take the string
**"I am user $myname" which has to be bandit23**
and then convert it to md5sum and then only take the first part of the total string
so the md5sum for the bandit23 string will be as follows:
**8ca319486bfbbc3663ea0fbe81326349**
so now as the last line suggests that the content will be stored in the **tmp** directory and the **same hash as the file name**
so lets check the tmp directory with this file name 
**/tmp/8ca319486bfbbc3663ea0fbe81326349**
and when doing **cat** we were able to see the contents of that file.
and this is the time when it got the answer easily other wise when i tried for the first time this file that we did cat was not at all readable also so i had to try various ways to see that file as the permissions were only to bandit23 and whole tmp folder was not accessible by bandit22.
so the password **0Zf11ioIjMVN551jX3CmStKLYqjk54Ga**
the photos for the same are as follows:![[Pasted image 20250106102330.png]]
