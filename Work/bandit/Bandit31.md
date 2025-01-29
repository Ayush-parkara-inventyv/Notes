same doing git clone 
 **git clone "ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo"**
 a bit of revise of the command and use password from [[Bandit30]]
 first do read the readme file and it suggests that we have to upload a file to the remote repository
 ![[Pasted image 20250107172345.png]]
 so lets start doing that 
 first make a file named **key.txt**
 then it wont allow us as it has gitignore so forcefully add the file 
 **git add -f key.txt**
 then set username and gmail
 and then do the commit
 **git commit -m "Second Commit"**
 ![[Pasted image 20250107172618.png]]
 then after commit check the graph using the same command used previously
![[Pasted image 20250107172719.png]]
and then at last push the same to the **branch master**
**git push -u origin master**
![[Pasted image 20250107172801.png]]
we got the password:**3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K**
