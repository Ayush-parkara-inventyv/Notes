ssh to the bandit8
**ssh bandit8@bandit.labs.overthewire.org -p 2220**
hint is that the file **data.txt** has so many lines but this line comes only once
i tried various command in which i got a bit of hint in which the duplicated lines were this
![[Pasted image 20241218142548.png]]
**uniq -d data.txt**
now i have to sort the file using **sort** and then use the **uniq** for the purpose of giving this duplicates and **|** piping to feed sort to the uniq and then check for output
u in uniq stand for unique lines
so the command is 
**sort data.txt | uniq -u**
and the output i got is **4CKMh1JI91bUIZZPXDqGanal4xvAg0JM**
password: **4CKMh1JI91bUIZZPXDqGanal4xvAg0JM**
