ssh to bandit10
**ssh bandit10@bandit.labs.overthewire.org -p 2220**
as the hint suggests it is base64 encoded data so the only thing to do is to decode it by base64
**base64 -d** is the commnd to decode the text
and we will cat the file and then pipeline it to decode the text
**cat data.txt | base64 -d**
![[Pasted image 20241218155950.png]]
password:**dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr**
