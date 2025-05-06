
### **Forked Repository (Fork Repo) - Quick Note**

A **forked repository** is a copy of an existing GitHub repository under a different account. Forking allows developers to experiment with changes without affecting the original repository.

### **Key Points:**

- Used to contribute to open-source projects.
- Enables working on modifications **without direct access** to the original repo.
- Maintains a **link** to the original repo, allowing updates from it.

### **Forking Process:**

1. Go to the **GitHub repository** you want to fork.
2. Click the **"Fork"** button (top right).
3. The repo is copied to **your GitHub account**.
4. You can now **clone**, edit, and submit a **Pull Request (PR)** to propose changes.

### **Syncing with the Original Repo:**

To keep your fork up-to-date with the original repo:

```bash
git remote add upstream https://github.com/original/repo.git  
git fetch upstream  
git merge upstream/main  
git push origin main  
```
