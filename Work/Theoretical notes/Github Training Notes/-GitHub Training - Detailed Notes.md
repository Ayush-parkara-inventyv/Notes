## **Introduction to GitHub**

GitHub is a cloud-based platform that facilitates version control using Git. It enables developers to collaborate, track changes, and maintain code repositories efficiently.

## **Git vs GitHub**

- **Git**: A version control system that allows tracking changes in code over time.
- **GitHub**: A web-based service that hosts Git repositories, allowing collaboration.
- **Analogy**:
    - Git is like recording different movie scenes.
    - GitHub is like uploading the finished movie to YouTube for collaboration.

---

## **Git Basics**

### **Installing Git**

#### Windows

1. Download Git from [git-scm.com](https://git-scm.com/download/win).
2. Install with default settings and add Git to the system PATH.
3. Verify installation using:
    
    ```bash
    git --version
    ```
    

#### Linux

4. Update system packages:
    
    ```bash
    sudo apt update && sudo apt upgrade
    ```
    
5. Install Git:
    
    ```bash
    sudo apt install git
    ```
    
6. Verify installation:
    
    ```bash
    git --version
    ```
    
## **Configuring Git**

### **Set Up User Details**

Configure user identity to track changes:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

Verify settings:

```bash
git config --get user.name
git config --get user.email
```

### **Ignore Unwanted Files**

For macOS, prevent `.DS_Store` files from appearing in commits:

```bash
echo .DS_Store >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

---

## **Working with Repositories**

### **Creating a Repository**

```bash
git init my_project
cd my_project
```

### **Adding Files to Staging Area**

```bash
git add filename  # Adds a specific file
git add .         # Adds all modified files
```

### **Committing Changes**

```bash
git commit -m "Commit message describing changes"
```

### **Checking the Status**

```bash
git status  # Shows modified files
```

### **Viewing Commit History**

```bash
git log
git log --oneline --graph --all  # Visual representation
```

---

## **Working with Remote Repositories (GitHub)**

### **Connecting Local Repo to GitHub**

10. Create a new repo on GitHub.
11. Link the local repo:
    
    ```bash
    git remote add origin https://github.com/username/repo.git
    ```
    

### **Pushing Local Changes to GitHub**

```bash
git push -u origin main
```

### **Pulling Latest Changes from GitHub**

```bash
git pull origin main
```

### **Cloning a GitHub Repository**

```bash
git clone https://github.com/username/repo.git
```

---

## **Branching in Git**

### **What is Branching?**

Branching allows parallel development without affecting the main codebase.

### **Creating a New Branch**

```bash
git branch feature-branch
```

### **Switching Between Branches**

```bash
git checkout feature-branch
```

OR

```bash
git switch feature-branch
```

### **Creating and Switching to a Branch**

```bash
git checkout -b feature-branch
```

### **Listing Branches**

```bash
git branch
```

### **Deleting a Branch**

```bash
git branch -d feature-branch  # If merged
git branch -D feature-branch  # Force delete
```

---

## **Merging Branches**

### **Basic Merge Process**

12. Switch to `main` branch:
    
    ```bash
    git checkout main
    ```
    
13. Merge `feature-branch` into `main`:
    
    ```bash
    git merge feature-branch
    ```
    

### **Handling Merge Conflicts**

14. Open conflicting file and manually resolve issues.
15. Mark file as resolved:
    
    ```bash
    git add conflicted-file.txt
    ```
    
16. Complete the merge:
    
    ```bash
    git commit -m "Resolved merge conflict"
    ```
    

### **Undoing a Merge**

Abort an in-progress merge:

```bash
git merge --abort
```

---

## **Working with Remote Branches**

### **Pushing a New Branch to GitHub**

```bash
git push origin feature-branch
```

### **Fetching Remote Changes**

```bash
git fetch origin
```

### **Pulling a Remote Branch**

```bash
git pull origin feature-branch
```

### **Deleting a Remote Branch**

```bash
git push origin --delete feature-branch
```

---

## **Git Workflow Example**

17. **Clone the Repository**
    
    ```bash
    git clone https://github.com/user/repo.git
    cd repo
    ```
    
18. **Create and Switch to a New Branch**
    
    ```bash
    git checkout -b feature-x
    ```
    
19. **Make Changes and Commit**
    
    ```bash
    echo "New feature" > feature.txt
    git add feature.txt
    git commit -m "Added new feature"
    ```
    
20. **Push Branch to GitHub**
    
    ```bash
    git push origin feature-x
    ```
    
21. **Create a Pull Request on GitHub**
22. **Merge Changes into `main` Branch**
    
    ```bash
    git checkout main
    git merge feature-x
    ```
    
23. **Delete Merged Branch**
    
    ```bash
    git branch -d feature-x
    git push origin --delete feature-x
    ```
    

---

## **Resolving Merge Conflicts**

### **Steps to Resolve a Merge Conflict**

24. Identify conflict using:
    
    ```bash
    git status
    ```
    
25. Open conflicting files and manually edit content.
26. Stage the resolved files:
    
    ```bash
    git add resolved-file.txt
    ```
    
27. Complete the merge:
    
    ```bash
    git commit -m "Resolved merge conflict"
    ```
    

---

## **Conclusion**

- Git allows efficient version control and collaboration.
- GitHub provides a platform to manage and share repositories.
- Understanding branching, merging, and conflict resolution is crucial.
- Regular practice and use of Git commands will enhance workflow efficiency.
