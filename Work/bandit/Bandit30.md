clone the git link using the password from the [[Bandit29]]
so here lets do the same steps 
**cat README.md**
so lets check the whole graph
**git log --graph --all**
![[Pasted image 20250107170554.png]]
so as you can see there is not commit to the readme file and also it is empty so this time there is no meaning to search anything related readme
so go to the **.git** directory
Now after going there as we know that packed-refs are the references for the branches or users that did the commits so lets check that first
**cat packed-refs**
![[Pasted image 20250107170938.png]]
so here we got something so there is a tag named secret 
i hope we will get something from that tag
so lets check it
go to repo and then check the secret
**git checkout secret**
so we got an error that we are not able to read the tree so we will not be able to do checkout then umm ......
oohhhh as it is a tag lets use **git show**
**git show secret**
![[Pasted image 20250107171237.png]]
and here we are .....
password for next level:**fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy**
