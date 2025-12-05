# GitHub Project Setup Guide - (GitHub to VSCode Workflow)

## Table of Contents
- [Project Setup Overview](#project-setup-overview)
- [Create the Repository on GitHub](#1-create-the-repository-on-github)
- [Clone the Repository Locally](#2-clone-the-repository-locally)
- [Confirm Git Setup](#3-confirm-git-setup)
- [Initialise Node and Install Dependencies](#4-initialise-node-and-install-dependencies)
- [Commit the Initial Setup](#5-commit-the-initial-setup)
- [Optional Folders](#optional-folders)
- [Common Dependencies](#common-dependencies)
- [Common Scripts](#common-scripts)
- [Troubleshooting Guide](#troubleshooting-guide)

---

# Project Setup Overview
This document explains how to create a GitHub repository, clone it to VS Code, initialise Node, and set up your development environment. It also includes common dependencies, scripts, and troubleshooting steps.

---

# 1. Create the Repository on GitHub
1. Log into your GitHub account.
2. Go to **Repositories → New**.
3. Select your **Owner** (your GitHub username).
4. Enter a **Repository Name**.
5. Add a **Description** (recommended).
6. Choose visibility:
   - **Public** → anyone can view the project.
   - **Private** → only invited users can see the repo.
7. Leave **Start with a Template → No Template**.
8. Enable **Add README**.
9. Enable **Add .gitignore** → choose **Node** from the dropdown.
10. Leave **Add licence → No licence**.
11. Click **Create Repository**.

---

# 2. Clone the Repository Locally
1. Copy the **SSH URL** from GitHub.
2. Open your terminal.
3. Navigate to where you store your projects:
   ```bash
   cd Desktop
   ```
4. Clone the repository:
   ```bash
   git clone git@github.com:your-username/your-repo.git
   ```
5. Type the name of the repo folder:
   ```bash
   cd your-repo
   ```
6. Open VS Code:
   ```bash
   code .
   ```

---

# 3. Confirm Git Setup
1. Check you're on the correct branch:
    ```bash
    git branch
    ```
    You should see:
    ```bash
    * main
    ```

2. Open **.gitignore** and confirm it contains:
    ```
    node_modules/
    .env
    .DS_Store
    ```
    Add anything missing.

3. If you made changes to your **.gitignore** file, you should:
    ```
    git add .gitignore
    git commit -m "update .gitignore file"
    git push origin main
    ```

---

# 4. Initialise Node and Install Dependencies
1. Create `package.json`:
   ```bash
   npm init -y
   ```
2. Install `node_modules`:
    ```bash
    npm install node
    ```
3. Install standard dependencies (e.g. Express):
   ```bash
   npm install express
   ```
4. Install dev dependencies (e.g. Nodemon):
   ```bash
   npm install -D nodemon
   ```

---

# 5. Commit the Initial Setup
1. Stage relevant files:
   ```bash
   git add package.json package-lock.json
   ```
2. Commit:
   ```bash
   git commit -m "setup project and install dependencies"
   ```
3. Push to GitHub:
   ```bash
   git push origin main
   ```

---

# Optional Folders
You may want to include additional folders depending on your project structure:

### `src/`
Used for application code.

### `tests/`
Used for Jest or other testing frameworks.

### `public/` or `assets/`
Used for frontend assets such as images or styles.

### `config/`
Used for configuration files, e.g. database or app config.

### `controllers/`, `models/`, `routes/`
Common folders for Express MVC structure.

These folders are optional — create them as needed.

---

# Common Dependencies

## Standard Dependencies (`npm install`)
- `express`
- `cors`
- `dotenv`
- `mongoose`
- `pg`
- `axios`
- `bcrypt`
- `jsonwebtoken`

## Dev Dependencies (`npm install -D`)
- `nodemon`
- `jest`
- `supertest`
- `eslint`
- `eslint-config-standard`
- `eslint-plugin-import`
- `eslint-plugin-node`
- `eslint-plugin-promise`
- `prettier`

---

# Common Scripts
Add these inside your `package.json` under `"scripts"`:

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "test": "jest",
  "lint": "eslint .",
  "format": "prettier --write ."
}
```

---

# Troubleshooting Guide

## ❗ Remote Origin Already Exists
**Error:**
```
error: remote origin already exists
```
**Fix:**
```bash
git remote remove origin
git remote add origin git@github.com:your-username/your-repo.git
```

---

## ❗ Merge Conflicts
**When it happens:**
- Running `git pull`
- Two people edited the same file

**Fix:**
1. Open files marked with conflict indicators:
```
Incoming changes
```
2. Manually choose what to keep.
3. Stage the resolved files:
```bash
git add .
```
4. Complete merge:
```bash
git commit
```

---

## ❗ Wrong Branch / Need to Switch Branch
Check current branch:
```bash
git branch
```
Create and switch to a new branch:
```bash
git checkout -b feature-branch
```
Switch to an existing branch:
```bash
git checkout main
```

---

## ❗ Accidentally Committed node_modules
Remove it from Git history:
```bash
git rm -r --cached node_modules
git add .gitignore
git commit -m "fix: remove node_modules from repo"
```
Push the fix:
```bash
git push origin main
```

---

## ❗ Push Rejected (Remote Has New Commits)
Error message:
```
! [rejected] main -> main (non-fast-forward)
```
Fix:
```bash
git pull --rebase origin main
git push origin main
```

---

## ❗ Local Changes You Want to Undo
Before using these commands, it's important to understand a key Git term:

### 🔍 **What is HEAD?**
`HEAD` is Git's way of saying **“your current position in the project history.”**
- It represents the **latest commit on the branch you're currently on**.
- When you run commands like `git reset HEAD~1`, you are telling Git to move your current position.

To see where HEAD is pointing:
```bash
git log --oneline --decorate
```
You’ll see something like:
```
3fa12c1 (HEAD -> main) Added login route
```
This means HEAD is pointing to the latest commit on the `main` branch.

---

Git provides three different ways to undo changes, each serving a different purpose. Use the right one depending on whether the change is committed or uncommitted, and whether you want to modify history.

### **1. `git restore` — Undo uncommitted changes (Safest)**
Use this when you want to discard changes in your working directory *before* committing.

Discard changes in a single file:
```bash
git restore <file>
```
Discard all uncommitted changes:
```bash
git restore .
```
Restore a deleted file:
```bash
git restore <file>
```

### **2. `git reset` — Move HEAD and optionally modify the staging area**
`reset` is powerful and can rewrite history. Three main types:

#### **Soft reset (keeps changes staged):**
Useful when you want to redo the last commit.
```bash
git reset --soft HEAD~1
```

#### **Mixed reset (default, keeps changes unstaged):**
Useful when you want to unstage changes but keep them in your working directory.
```bash
git reset HEAD~1
```

#### **Hard reset (dangerous — deletes changes):**
Completely wipes working directory changes and moves HEAD.
```bash
git reset --hard HEAD
```

Restore remote to match local after hard reset:
```bash
git push origin main --force
```
(Only use when you *understand the impact*.)

### **3. `git revert` — Safely undo a commit without rewriting history**
Use this to undo a commit that has already been pushed to a shared repo.
Creates a new commit that reverses the old one.

### 🔍 **How to find the commit ID**
A commit ID is the unique SHA hash for a commit.
Run:
```bash
git log --oneline
```
Example output:
```
3fa12c1 Added login route
91be22f Initial setup
```
Here, `3fa12c1` is the commit ID.

### 🔄 Revert a specific commit
```bash
git revert <commit-id>
```

### 🔄 Revert multiple commits
```bash
git revert HEAD~3..HEAD
```

**Best for team projects** because it does *not* rewrite history.

---
Use this to undo a commit that has already been pushed to a shared repo.
Creates a new commit that reverses the old one.

Revert a specific commit:
```bash
git revert <commit-id>
```
Revert multiple commits:
```bash
git revert HEAD~3..HEAD
```

**Best for team projects** because it does *not* rewrite history.

---

This README template can be copied into any project to guide users through setup, dependencies, scripts, and common troubleshooting steps.
