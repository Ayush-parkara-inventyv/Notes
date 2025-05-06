
### Creating branches with `git branch`

This is the most easiest and obvious way to create a new branch. We simply use the command,

```bash
#syntax
git branch <branch_name> 

#example
git branch feature1 #this will create a branch named 'feature1'

#If you wish to see the branches that are available
git branch
```

![](https://i.imgur.com/9gNQGdJ.png)
### Creating branches with `git checkout`

Now the other common method used by many is the use of the `git checkout` command which offers the same functionality with one slight change.

```bash
#syntax
git checkout -b <branch_name> 

#example
git checkout -b feature2 #this will create a branch named 'feature1'

#If you wish to see the branches that are available
git branch
```

### Understanding the workflow

#### Step 1
Now let us try to create a dummy project and go through a simple scenario.


![](https://i.imgur.com/utw2rbn.png)
#### Step 2
Now let us try to add a new feature to out code by creating a new branch,

```bash
#creating a new branch
git branch feature1

#checking the branches
git branch

#switching to that branch
git checkout feature1

#checking log
git log --all --graph

```


![](https://i.imgur.com/8Yf7ho4.png)


#### Step 3

Let's see what files are shown in both the branches and see the difference.

```bash
#to see the files in the feature1 branch
git checkout feature1
ls

#to see the files in the main branch
git checkout main
ls

```


![](https://i.imgur.com/GEEH0Mf.png)

![](https://i.imgur.com/9JZpGwR.png)

```bash
#ensuring we are in the main branch
git branch

#making a hotfix in the main branch
echo "hotfix 1.0" >> hotfix.txt
git add hotfix.txt
git commit -m "fixed a bug"

#Checking out the logs
git log --all --graph

```



![](https://i.imgur.com/1MFMpAD.png)


#### Step 4

Now, lets try to checkout what is going on in the `feature1` branch,

```bash
#switching to feature1 branch
git checkout feature1

#checking out the files here
ls

#Checking out the logs
git log --all --graph
```


![](https://i.imgur.com/F4UaGSg.png)

### Connecting `main` branch

```bash
#adding repo remote to git
git remote add origin https://github.com/ayush.parkara.inventyv/website-flow.git

#switching to main branch
git checkout main

#pushing the main branch
git push origin main #we get an error here, we will unpack this later

#performing fetch
git fetch

#trying to push the main branch again
git push origin main #we get an error here again

#trying again but forced update this time
git push origin main -f
```

![](https://i.imgur.com/Et6Ou2a.png)

### Connecting `feature1` branch

Given that we have pushed our `main` branch upstream now its time to do the same for our `feature1` branch. It is a simple command,

```bash
#pushing the feature1 branch
git push origin feature1
```

Now if we perform `git log --all --graph` we will see,

![](https://i.imgur.com/b7Gke81.png)
## Merging

Creating branches is all fun and games, until it comes to merging branches, now the command to do so is very simple but it requires a lot of logic. Merging of two branches takes place when you are ready to integrate your feature back into the main branch.

Merging happens from the branch you are currently pointed into, meaning if you are say inside the `main` branch, and use the command `git merge` on the `feature1` branch, then the `feature1` branch would merge back into `main`.

```bash
#ensuring we are in the main branch
git checkout main

#merging two branches
git merge feature1 -m "merging feature1 back to main"

#checking the files now
ls
```


![](https://i.imgur.com/mySmnF6.png)


```bash
#Checking out the logs 
git log --all --graph
```

![](https://i.imgur.com/L93cEJM.png)

### Creating a merge conflict

In order to get real familiar with merge conflicts, the next section will simulate a conflict to later examine and resolve. The example will be using a Unix-like command-line Git interface to execute the example simulation.

```bash
mkdir git-merge-test
cd git-merge-test
git init .
echo "this is some content to mess with" > merge.txt
git add merge.txt
git commit -m "we are commiting the inital content"
```

![](https://i.imgur.com/SL36FTc.png)

This code example executes a sequence of commands that accomplish the following.

- Create a new directory named `git-merge-test,` change to that directory, and initialize it as a new Git repo.
- Create a new text file `merge.txt` with some content in it.  
- Add `merge.txt` to the repo and commit it.

Now we have a new repo with one branch `main` and a file `merge.txt` with content in it. Next, we will create a new branch to use as the conflicting merge.

```bash
git checkout -b new_branch_to_merge_later
echo "totally different content to merge later" > merge.txt
git add .
git commit -m "edited the content of merge.txt to cause a conflict"
```

![](https://i.imgur.com/IYIItJB.png)


![](https://i.imgur.com/2302hFh.png)

With this new branch: `new_branch_to_merge_later` we have created a commit that overrides the content of `merge.txt`

```bash
git checkout main
echo "content to append" >> merge.txt
git add .
git commit -m "appended content to merge.txt"
```

![](https://i.imgur.com/Yb5JvbF.png)

```bash
git merge new_branch_to_merge_later
```

### How to identify merge conflicts

As we have experienced from the proceeding example, Git will produce some descriptive output letting us know that a CONFLICT has occured. We can gain further insight by running the `git status` command

```bash
git status
```

```bash
cat merge.txt
```

![](https://i.imgur.com/uuTZFUl.png)
### How to resolve merge conflicts using the command line

The most direct way to resolve a merge conflict is to edit the conflicted file. Open the `merge.txt` file in your favorite editor. For our example lets simply remove all the conflict dividers. The modified `merge.txt` content should then look like:

```bash
this is some content to mess with
content to append
totally different content to merge later
```

Once the file has been edited use `git add merge.txt` to stage the new merged content. To finalize the merge create a new commit by executing:

```bash
git commit -m "merged and resolved the conflict in merge.txt"

git log --all --graph
```


![](https://i.imgur.com/FtdRczF.png)


Practice branching and commands


![image](https://github.com/user-attachments/assets/0261f00c-09a8-4b19-b40a-d336605707ac)

![image](https://github.com/user-attachments/assets/7d372d2d-ef7e-4857-8938-b7416379f3d0)

	
