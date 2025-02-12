# Git Guide

Git is a distributed version control system that allows you to track changes in your code, manage multiple versions of your project, and collaborate efficiently with others. This guide covers the basics of Git—from initializing a repository to working with branches, viewing commit history, and syncing with remote repositories.

---

## Table of Contents

- [1. Introduction to Git](#1-introduction-to-git)
- [2. Installing Git](#2-installing-git)
- [3. Basic Git Commands](#3-basic-git-commands)
  - [3.1. Initializing a Repository](#31-initializing-a-repository)
  - [3.2. Checking Repository Status](#32-checking-repository-status)
  - [3.3. Staging Changes](#33-staging-changes)
  - [3.4. Committing Changes](#34-committing-changes)
  - [3.5. Viewing Commit History](#35-viewing-commit-history)
  - [3.6. Working with Remote Repositories](#36-working-with-remote-repositories)
- [4. Working with Branches](#4-working-with-branches)
  - [4.1. Creating and Switching Branches](#41-creating-and-switching-branches)
  - [4.2. Merging Branches](#42-merging-branches)
  - [4.3. Resolving Merge Conflicts](#43-resolving-merge-conflicts)
- [5. Useful Tips](#5-useful-tips)
- [Conclusion](#conclusion)

---

## 1. Introduction to Git

Git is a version control system that allows you to:

- **Keep a history** of all changes in your project.
- **Switch easily** between different versions of your code.
- **Collaborate effectively** with team members.
- **Resolve conflicts** when merging changes.

---

## 2. Installing Git

### For Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git -y
```

### For macOS

Install Git using Homebrew:

```bash
brew install git
```

### For Windows

Download and install [Git for Windows](https://git-scm.com/download/win).

After installation, verify the Git version:

```bash
git --version
```

---

## 3. Basic Git Commands

### 3.1. Initializing a Repository

To start tracking your project with Git, initialize a repository in your project directory:

```bash
git init
```

This creates a hidden `.git` directory that stores your repository data.

### 3.2. Checking Repository Status

View the current status of your working directory and staging area:

```bash
git status
```

This command shows which files have been modified, which are staged, and which remain untracked.

### 3.3. Staging Changes

Add changes to the staging area to prepare them for a commit:

- To add a specific file:
  ```bash
  git add filename.js
  ```
- To add all changes:
  ```bash
  git add .
  ```

### 3.4. Committing Changes

Create a commit with a descriptive message:

```bash
git commit -m "A brief description of the changes"
```

### 3.5. Viewing Commit History

To see the history of commits:

```bash
git log
```

Press `q` to exit the log view.

### 3.6. Working with Remote Repositories

#### Adding a Remote Repository

Link your local repository to a remote one (e.g., on GitHub):

```bash
git remote add origin git@github.com:yourusername/yourrepo.git
```

#### Checking Remote Configuration

Verify the remote URLs:

```bash
git remote -v
```

#### Pulling Changes

Fetch and merge changes from the remote repository:

```bash
git pull origin main
```

#### Pushing Changes

Send your local commits to the remote repository:

```bash
git push origin main
```

---

## 4. Working with Branches

### 4.1. Creating and Switching Branches

- **Creating a new branch and switching to it:**
  ```bash
  git checkout -b new-feature
  ```
- **Switching to an existing branch:**
  ```bash
  git checkout main
  ```

### 4.2. Merging Branches

Merge changes from one branch (e.g., `new-feature`) into the current branch (e.g., `main`):

```bash
git merge new-feature
```

### 4.3. Resolving Merge Conflicts

If conflicts arise during a merge, Git will mark the conflicted files. To resolve:

1. **Edit the files** to resolve conflicts.
2. **Stage the resolved files:**
   ```bash
   git add conflicted-file.js
   ```
3. **Commit the merge:**
   ```bash
   git commit -m "Resolved merge conflicts"
   ```

---

## 5. Useful Tips

- **.gitignore:**  
  Create a `.gitignore` file to exclude files and directories (e.g., `node_modules`, logs) from being tracked.
- **Commit Frequently:**  
  Commit changes often with clear, descriptive messages. This makes it easier to track progress and revert if necessary.
- **Branching Strategy:**  
  Use branches for new features or bug fixes. This isolates changes and simplifies merging.
- **Status Check:**  
  Always run `git status` before committing to ensure you know what will be included.
- **Graphical Tools:**  
  Many editors (like Visual Studio Code) offer built-in Git tools to visualize changes and manage branches.

---

God Save The JS!
