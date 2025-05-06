use the password from 5 and **ssh** next bandit
**ssh bandit6@bandit.labs.overthewire.org -p 2220**
here i got only hidden files by doing **ls -a**
i got 3 hidden files
and then saw the contents for each file
![[Pasted image 20241218132132.png]]
then in **cat ./.profile**
i got the following content
	![[Pasted image 20241218132235.png]]
	got few interesting things
	![[Pasted image 20241218132559.png]]
	also this
	![[Pasted image 20241218132621.png]]
	in **bandit7** i saw data.txt
	and in that the permission is not given
	![[Pasted image 20241218132833.png]]
	some trial and errors
	![[Pasted image 20241218133430.png]]
	then i used all the hints as the ownership adding was not known to me
	so i used **-user** and **-group** in find to give owner ship **/** for checking the directories and **-size** to find the file of **size 33c**
	so the command i used is **find**
	**find / -user bandit7 -group bandit6 -size 33c**
	now there was a big list in which all files were not accessible except 1 file
	![[Pasted image 20241218135636.png]]
	**filename: /var/lib/dpkg/info/bandit7.password**
	so then **cat** the file 
	**cat /var/lib/dpkg/info/bandit7.password**
	![[Pasted image 20241218135742.png]]
	**password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj**