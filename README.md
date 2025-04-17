# Removing a Commit from Main While Keeping a Backup

This guide explains how to remove a commit from the `main` branch while keeping it safely stored in a separate backup branch.

## **Step 1: Create a Backup Branch**
Before removing the commit, create a backup branch that retains the commit:

```sh
# Switch to main and create a backup branch from the commit
# Use the commit hash that you want to remove from the main branch
git checkout -b backup-removed-commit b48671c072dc598a1c09af265604c6326c0b7f27

# Push the backup branch to remote
git push origin backup-removed-commit
```

This ensures that the commit remains accessible even after it is removed from `main`.

---

## **Step 2: Remove the Commit from `main`**
Switch back to the `main` branch:

```sh
git checkout main
```

Start an interactive rebase to remove the commit:

```sh
# Enter the previous commit hash of the targetted commit. Make sure you only select the previous commit 
git rebase -i --rebase-merges <commit_before_b48671c072dc598a1c09af265604c6326c0b7f27>
```

In the interactive rebase editor:
1. Locate the line containing `b48671c072dc598a1c09af265604c6326c0b7f27`. In this case, the commit you want to remove
2. Delete that line.
3. Save and close the editor.

This will remove the commit while keeping the rest of the history intact.

---

## **Step 3: Force Push to Update Remote**
Since this process rewrites history, you need to force push the updated `main` branch:

```sh
git push origin main --force
```

⚠️ **Warning:** This operation rewrites history. If others are working on the same branch, coordinate with them before pushing.

---

## **Final Result**
- The commit `b48671c072dc598a1c09af265604c6326c0b7f27` is no longer in `main`.
- The commit is safely stored in the `backup-removed-commit` branch.
- If needed, you can restore the commit from the backup:

```sh
git checkout backup-removed-commit
```

---

## **Restoring the Commit to Main (If Needed)**
If you ever need to restore the commit back to `main`, use:

```sh
git checkout main
git cherry-pick b48671c072dc598a1c09af265604c6326c0b7f27
```

This will reapply the commit to `main` without affecting other changes.

---











<br><br><br>
















# 🧹 Removing Untracked Files in Git  
*Clean up the mess and keep your repo tidy!*

Sometimes your working directory contains untracked files that clutter your repo. You can remove them using `git clean`.


### **1. Check What Will Be Deleted**
Before running any destructive command, check which untracked files would be removed:

```sh
git clean -n
```

This will show a **preview** of untracked files and directories that would be deleted.

---

### **2. Remove Untracked Files**
To delete the untracked files (but not directories):

```sh
git clean -f
```

---

### **3. Remove Untracked Files and Directories**
If you want to remove both untracked files and directories:

```sh
git clean -fd
```

---

### **4. Remove Ignored and Untracked Files**
To remove **everything** including ignored files:

```sh
git clean -xfd
```

- `-x`: Removes ignored files (those listed in `.gitignore`)
- `-f`: Forces the deletion (required)
- `-d`: Deletes untracked directories

⚠️ **Warning:** These commands are irreversible. Always run `git clean -n` first to preview what will be removed!

<br><br><br>



## 🔁 Git Pull with Rebase

To keep a clean and linear Git history, we recommend using `--rebase` when pulling changes from a remote branch.

### ✅ One-Time Pull with Rebase

To pull changes from a remote branch and rebase your local changes on top:

```bash
git pull --rebase origin feature/static-html
```

<br><br><br>

## Keep your branch safe from falling off the tree 😁 🚀
