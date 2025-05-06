ssh to bandit21 using password from [[Bandit20]]
Now as the hint suggests that the cron is used and cron is a job scheduler 
so first it gave us a directory to check the cronjobs that are already scheduled 
so i visited the directory **/etc/cron.d**
now after visiting the directory i got this results 
i also did cat to all the cronjobs that were there in that folder
![[Pasted image 20250103160821.png]]
now as the cronjob suggests the location i visited the location and checked the file 
cmd:**cat /usr/bin/cronjob_bandit22.sh**
![[Pasted image 20250103160910.png]]
this shows that at every restart and every minute the cronjob does the saving of password of bandit22 into the temp directory that is mentioned there
so lets check the same directory
**cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv**
and this is the output![[Pasted image 20250103161102.png]]
and bingo!!
we got the password of bandit22
password:**tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q**
