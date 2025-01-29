ssh to bandit23 using the password from [[Bandit22]]
okay so in this also start by checking the **/etc/cron.d** and then check the **cronjob_bandit24** and it seems to run a code with loop that executes all the files from the folder mentioned
so the description for the same is as follows at the last
so after understanding the working of the cronjob i understood that if there is a script that stores the password of bandit24 into some file just like 23 it will be easy as the file will get executed and then deleted also which will remove the traces and as the execution process is checked it is done on **every minute and every restart**

So i took the same script from the bandit23 cronjob and made a new file inside the directory that was mentioned in the cronjob and before that i already checked the permissions of that folder which were write and execute which is a much easy part to save and get the file executed.
so made the script as follows:
 **nano /var/spool/bandit24/foo/test.sh** 
 **#!/bin/bash myname=$(whoami) 
 mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
 echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
 cat /etc/bandit_pass/$myname > /tmp/$mytarget**
 and then i used this script to get executed after 1 minute
 which will use **bandit24** as the **name** and then will check the password and store it on the **md5sum string as a file name in tmp**
 so then after 1 minute i checked the script that i created and that was not there so the script would have been executed so i checked the **tmp** directory with the **string obtained from the md5sum**
 so the command was **cat /tmp/ee4ee1703b083edac9f8183e4ae70293**
 The password i got from that **gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8**
 
 
 Photos for the same:
![[Pasted image 20250106110721.png]]
sort of task performed by the cronjob of bandit24
![[Pasted image 20250106110822.png]]
this image shows the saving of the script and then calculating the file location and then checking it for password.
Explanation for the [[cronjob_bandit24 Explanation]]






