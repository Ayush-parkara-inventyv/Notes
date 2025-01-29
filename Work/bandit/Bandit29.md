Use the same method to do git clone and use the password from the [[Bandit28]]
**git clone "ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo"**
now after doing that check the readme file 
![[Pasted image 20250107152108.png]]
so these are the contents in the readme file so lets check for previous commits and changes to the file
**git log -p --follow -- README.md**
![[Pasted image 20250107154443.png]]
so this is the output i got from the command
now i really searched everything but i got few suggestions from my trainer that one thing we should use which is more better is
**git log --graph --all**
![[Pasted image 20250107165905.png]]
see this shows that we need to check the branch dev which will contain the password for the next level
so lets change the branch to dev and then check the logs
**git checkout dev**
and then lets do **cat** first if we dont get the password then will check further
**cat README.md**
and here you are hehehehehhe!!!!
![[Pasted image 20250107170312.png]]
the password for next level is **qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL**
