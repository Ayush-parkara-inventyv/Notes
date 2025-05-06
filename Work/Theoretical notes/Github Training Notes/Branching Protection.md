**Branch Protection** in GitHub ensures that important branches (like `main` or `develop`) are safeguarded against accidental changes, deletions, or unauthorized modifications.

---

## **Why Use Branch Protection?**

✅ Prevents accidental **deletion or direct modification** of critical branches.  
✅ Enforces **code review policies** before merging.  
✅ Ensures **tests and checks** pass before accepting changes.  
✅ Restricts **who can push or merge changes**.

---

## **Branch Protection Rules in GitHub**

### **1. Enforce Code Reviews**

- Requires at least **one approved review** before merging a Pull Request (PR).
- Prevents merging without approval.

### **2. Require Status Checks to Pass**

- Ensures that CI/CD tests (like **GitHub Actions, Jenkins, or Travis CI**) pass before merging.
- Prevents broken code from entering the main branch.

### **3. Restrict Who Can Push**

- Limits push access to **specific users or teams**.
- Maintains repository integrity by restricting changes.

### **4. Require Branch to be Up-to-Date**

- Forces developers to **sync with the latest `main` branch** before merging.
- Reduces conflicts and merge issues.

### **5. Prevent Branch Deletion**

- Protects critical branches (`main`, `develop`, `release`) from being deleted.
- Ensures project stability.

---

## **How to Enable Branch Protection in GitHub**

1. Go to **GitHub Repository** → Click on **Settings**.
2. Navigate to **Branches** → **Branch Protection Rules**.
3. Click **"Add Rule"** and select the branch (e.g., `main`).
4. Configure protection settings:  
    ✅ Require pull requests before merging.  
    ✅ Require status checks to pass.  
    ✅ Restrict who can push to the branch.
5. Click **"Save Changes"**.

---

## **Best Practices**

✔ Protect `main` and `develop` branches.  
✔ Require at least **one review approval**.  
✔ Use **automated tests (CI/CD)** for quality control.  
✔ Keep branches **updated before merging**.  
✔ Grant push access only to **trusted contributors**.
