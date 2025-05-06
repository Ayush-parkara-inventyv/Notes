did **ssh** 
**ssh bandit5@bandit.labs.overthewire.org -p 2220**
in that folder i got various files
now the hind is the file is **human readable** **1033 bytes size** and **not executable**
so i got the hint as i used the same as previous
use **ls** with **-s 1033c**size 1033 and not executable file and **-h** human readable
![[Pasted image 20241218131017.png]]
then i used **find** and adding the size criteria
so the command used is **find ./* -size 1033c**
and then the **cd maybehere07** and then **cat** command to file2
**cat "spaces file2"**
![[Pasted image 20241218131442.png]]
then i tried the actual file that was shown in the find command
**cat ./.file2**
![[Pasted image 20241218131544.png]]
then i got the password
**password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG**
