### **Pull Request (PR)**

A **Pull Request (PR)** is a feature in GitHub (and other Git-based platforms) that allows developers to propose and review changes before merging them into the main codebase.

---

## **Steps to Create a Pull Request**

### **1. Fork or Clone the Repository (if applicable)**

- If working on a public repo, **fork** it.
- If working on a team project, **clone** the repo using:
    
    ```bash
    git clone https://github.com/user/repo.git
    cd repo
    ```
    

### **2. Create a New Branch**

- Create and switch to a new feature branch:
    
    ```bash
    git checkout -b feature-branch
    ```
    

### **3. Make Changes & Commit**

- Edit files and add them to staging:
    
    ```bash
    git add .
    ```
    
- Commit changes:
    
    ```bash
    git commit -m "Added new feature"
    ```
    

### **4. Push the Branch to GitHub**

- Push the feature branch to the remote repository:
    
    ```bash
    git push origin feature-branch
    ```
    

### **5. Open a Pull Request on GitHub**

- Go to the **GitHub repository**.
- Click on **"Compare & pull request"** next to the pushed branch.
- Add a **title and description** of changes.
- Select the **base branch** (usually `main` or `develop`).

### **6. Review and Discussion**

- Reviewers check the code and may request changes.
- Discussions and suggestions are handled in the PR comment section.

### **7. Approve and Merge the PR**

- If approved, merge using:
    - **Squash & Merge** (combines all commits into one).
    - **Rebase & Merge** (keeps history cleaner).
    - **Merge Commit** (keeps all commit history).
- Can be done via GitHub UI or command line:
    
    ```bash
    git checkout main
    git merge feature-branch
    ```
    

### **8. Delete the Feature Branch

- Once merged, delete the branch to keep the repository clean:
    
    ```bash
    git branch -d feature-branch
    git push origin --delete feature-branch
    ```
    

---

