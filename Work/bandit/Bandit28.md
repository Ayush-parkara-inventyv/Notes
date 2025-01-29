Use the same sort of command for the cloning of the repo of github and see the password for next level
command:
**git clone "ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo"**
and the same will get you the git repo
and then check the readme file for next level password
![[Pasted image 20250107150847.png]]
![[Pasted image 20250107150857.png]]
so first of all let me check that this **xxxxxx.....** is the password or the password is intentionally hidden
![[Pasted image 20250107151129.png]]
see this password doesnt work at all let me see what the scenario 
heyyyyy maybe the file was updated from the initial file lets check for some of older updates as it is a github repo it can have multiple commits
seeeeee!!!!!!
as i thought this file has been edited thrice see this 
the command for this is as follows:
**git log --follow -- README.md**
![[Pasted image 20250107151519.png]]
and then lets check the content that was changed and we got the final file of **readme**
command:**git log -p --follow -- README.md**
![[Pasted image 20250107151704.png]]
it just goes on and on so we have the password here
**4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7**
