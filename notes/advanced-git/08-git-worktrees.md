অবশ্যই। এবার এটাকে আগের **`05-merge-conflicts.md`**-এর মতোই clean, professional এবং GitHub-friendly Markdown format-এ সাজাচ্ছি।

তোমার project structure অনুযায়ী এই ফাইলটি হবে:

```text
08-git-worktrees.md
```

````markdown
# Git Worktrees

## Overview

**Git Worktree** is an advanced Git feature that allows you to work with multiple branches of the same repository simultaneously using separate working directories.

Instead of switching branches inside the same folder, you can create multiple folders where each folder is associated with a different branch.

> **Git Worktree = Multiple working directories for the same Git repository, allowing you to work on different branches at the same time.**

---

# What is a Git Worktree?

Suppose your project is:

```text
ETL-Pipeline/
````

You are currently working on:

```text
main
```

Suddenly, you need to fix an urgent bug without interrupting your current work.

With the traditional approach, you might need to:

```text
main
 ↓
stash changes
 ↓
switch to bugfix branch
 ↓
fix the bug
 ↓
switch back to main
 ↓
stash pop
```

This can become inconvenient, especially when you have many uncommitted changes.

With Git Worktree, you can create another directory:

```text
ETL-Pipeline/
│
├── main-project/
│      ↓
│     main
│
└── etl-bugfix/
       ↓
       bugfix/data-validation
```

Now both branches can be active at the same time.

---

# How Git Worktree Works

Imagine a repository containing:

```text
Repository
│
├── main
├── feature/login
└── bugfix/data-validation
```

With worktrees, these branches can be checked out into separate directories:

```text
Projects/
│
├── main-project/
│      ↓
│     main
│
├── login-feature/
│      ↓
│     feature/login
│
└── etl-bugfix/
       ↓
       bugfix/data-validation
```

Each directory has its own working files, while the worktrees belong to the same Git repository.

---

# `git switch` vs `git worktree`

This is one of the most important differences to understand.

## `git switch`

The `git switch` command changes the branch in the **current working directory**.

For example:

```bash
git switch feature
```

If the current directory is:

```text
Project/
    ↓
   main
```

after switching:

```text
Project/
    ↓
  feature
```

The same directory now represents a different branch.

---

## `git worktree`

With Git Worktree:

```text
Directory A → main
Directory B → feature
```

Both directories can remain active simultaneously.

Therefore:

```text
git switch
    ↓
Same directory
    ↓
Change the active branch


git worktree
    ↓
Multiple directories
    ↓
Multiple branches simultaneously
```

---

# Why Use Git Worktrees?

Consider a common development situation.

You are working on a large feature:

```text
main
 ↓
New feature development
```

You already have several uncommitted changes.

Suddenly:

```text
🚨 Critical production bug!
```

You need to fix the bug immediately.

---

## Traditional Approach

You might need to:

```bash
git stash
git switch bugfix
```

After fixing the bug:

```bash
git switch main
git stash pop
```

This temporarily removes your current changes.

---

## Worktree Approach

Create a separate worktree:

```text
main-project/
    ↓
Ongoing feature development

bugfix-project/
    ↓
Urgent bug fix
```

Now you can work on the bug without changing the current working directory.

> **Worktrees allow parallel development without interrupting your current work.**

---

# Creating a Worktree

The basic command is:

```bash
git worktree add <directory> <branch>
```

For example:

```bash
git worktree add etl-bugfix bugfix/data-validation
```

This creates:

```text
etl-bugfix/
    ↓
bugfix/data-validation
```

---

# Creating a New Branch and Worktree Together

If the branch does not already exist, you can create the branch and worktree in a single command:

```bash
git worktree add -b bugfix/data-validation etl-bugfix
```

Here:

```text
-b
 ↓
Create a new branch
```

and:

```text
etl-bugfix
 ↓
New worktree directory
```

So one command performs two tasks:

```text
Create branch
      +
Create worktree
```

---

# Practical Example

Suppose your current project is:

```text
ETL-Pipeline/
```

and you are working on:

```text
main
```

You suddenly need a separate branch for data validation fixes.

Run:

```bash
git worktree add -b bugfix/data-validation etl-bugfix
```

Your setup becomes:

```text
ETL-Pipeline/
│
├── main project
│      ↓
│     main
│
└── etl-bugfix/
       ↓
       bugfix/data-validation
```

Now open another terminal and run:

```bash
cd etl-bugfix
```

You can work on the bugfix branch without changing the main worktree.

---

# Viewing Existing Worktrees

To see all worktrees associated with the repository:

```bash
git worktree list
```

Example output:

```text
C:/Projects/ETL-Pipeline    abc1234 [main]
C:/Projects/etl-bugfix      def5678 [bugfix/data-validation]
```

This information tells you:

```text
Path
 ↓
Current commit
 ↓
Branch
```

---

# Removing a Worktree

When you finish working on a worktree, you can remove it using:

```bash
git worktree remove etl-bugfix
```

This removes the working directory:

```text
etl-bugfix/
```

However, there is an important distinction:

> **Removing a worktree does not delete its branch.**

For example:

```text
Worktree
   ↓
❌ Removed

Branch
   ↓
✅ Still exists
```

The branch:

```text
bugfix/data-validation
```

will remain available.

If you also want to delete the branch:

```bash
git branch -d bugfix/data-validation
```

---

# Worktree vs Branch

A branch and a worktree are not the same thing.

## Branch

A branch is a Git reference that points to a commit.

```text
Branch
   ↓
Git reference
   ↓
Points to a commit
```

## Worktree

A worktree is a physical working directory where a branch is checked out.

```text
Worktree
   ↓
Working directory
   ↓
Branch checked out here
```

Therefore:

```text
Branch ≠ Worktree
```

Removing a worktree does not automatically remove the branch.

---

# Important Worktree Rule

A branch is normally checked out in only one worktree at a time.

For example:

```text
Worktree A → main
```

Trying to check out the same branch in another worktree may result in Git preventing the operation.

A typical setup is therefore:

```text
Worktree A → main
Worktree B → feature
Worktree C → bugfix
```

Each worktree has its own active branch.

---

# When Are Worktrees Useful?

## 1. Multiple Feature Development

You may need to work on several features at the same time:

```text
main
 ↓
Feature A

feature/b
 ↓
Feature B
```

Using worktrees:

```text
project-main/
    ↓
main

project-feature-b/
    ↓
feature/b
```

Both can remain active simultaneously.

---

## 2. Urgent Bug Fixes

Suppose you are currently working on:

```text
feature/new-dashboard
```

and an urgent bug appears:

```text
bugfix/login
```

With worktrees:

```text
feature worktree
    ↓
Ongoing development

bugfix worktree
    ↓
Urgent bug fix
```

You do not need to interrupt the feature development.

---

## 3. Long-Running Processes

Suppose a long-running process is running:

```bash
python train.py
```

and it takes two hours.

You can keep it running in one worktree:

```text
Worktree A
    ↓
Long-running training
```

while using another worktree:

```text
Worktree B
    ↓
New development
```

This allows development to continue without interrupting the running process.

---

## 4. Code Review

Suppose you are working on:

```text
feature/payment
```

and need to review another developer's branch:

```text
feature/new-api
```

Instead of switching branches, create another worktree:

```text
project-main/
    ↓
Your current work

project-review/
    ↓
Review branch
```

You can now inspect and test the other branch independently.

---

# Advantages of Git Worktrees

## 1. Less Need for Stashing

You often do not need to stash uncommitted changes just to work on another branch.

---

## 2. Parallel Development

Multiple branches can be active simultaneously.

```text
Feature
Bugfix
Code Review
Testing
```

Each can have its own working directory.

---

## 3. Easy Context Switching

Instead of switching branches:

```text
Folder A → Feature
Folder B → Bugfix
Folder C → Review
```

Simply move between directories.

---

## 4. Independent Running Processes

A process running in one worktree does not require you to stop working elsewhere.

This is especially useful for:

* Long-running tests
* Builds
* Data processing
* Model training
* Development servers

---

# Disadvantages of Git Worktrees

Worktrees are powerful, but they are not always necessary.

## 1. More Disk Usage

Each worktree contains its own working files.

For example:

```text
Project = 10 GB
```

Multiple worktrees may increase the amount of disk space used by working copies.

However, Git's repository object database is generally shared between linked worktrees, so the entire Git history is not duplicated for every worktree.

---

## 2. Directory Management

Too many worktrees can become confusing:

```text
project/
project-feature/
project-bugfix/
project-review/
project-testing/
```

It can become difficult to remember which directory represents which branch.

Clear naming conventions help solve this problem.

---

# Cleaning Stale Worktree Metadata

Sometimes a worktree directory may be manually deleted instead of using:

```bash
git worktree remove
```

In that situation, Git may still have metadata referencing the old worktree.

To clean unused worktree metadata:

```bash
git worktree prune
```

This removes stale worktree information.

---

# Recommended Naming Convention

A useful naming convention is to make the directory name reflect the branch or purpose.

For example:

```text
project-main/
project-feature-auth/
project-bugfix-validation/
project-review-api/
```

This makes it easier to understand:

```text
Directory
    ↓
Purpose
    ↓
Branch
```

---

# Complete Worktree Workflow

A practical workflow might look like this:

```text
                    Repository
                        │
                        ▼
                       main
                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼
       feature/       bugfix/       review/
         auth        validation       api
          │             │             │
          ▼             ▼             ▼
      Worktree A    Worktree B    Worktree C
```

---

## Step 1: Create a Feature Worktree

```bash
git worktree add -b feature/auth ../project-auth
```

---

## Step 2: Create a Bugfix Worktree

```bash
git worktree add -b bugfix/validation ../project-validation
```

---

## Step 3: List All Worktrees

```bash
git worktree list
```

---

## Step 4: Enter a Worktree

```bash
cd ../project-validation
```

---

## Step 5: Work on the Branch

```bash
git status
git add .
git commit -m "Fix data validation"
```

---

## Step 6: Remove the Worktree

After finishing the work:

```bash
git worktree remove ../project-validation
```

---

## Step 7: Clean Stale Metadata

If necessary:

```bash
git worktree prune
```

---

# `git switch` vs `git worktree`

| Feature                      | `git switch`               | `git worktree`        |
| ---------------------------- | -------------------------- | --------------------- |
| Change branch                | ✅                          | ✅                     |
| Multiple branches active     | ❌                          | ✅                     |
| Multiple working directories | ❌                          | ✅                     |
| Stashing may be required     | ✅                          | Usually not           |
| Disk usage                   | Lower                      | Higher                |
| Parallel development         | Limited                    | Excellent             |
| Urgent bug fixes             | Less convenient            | Very useful           |
| Code review                  | Requires switching         | Separate directory    |
| Long-running processes       | Context switching required | Can run independently |

---

# Most Important Commands

## Create a Worktree

```bash
git worktree add <directory> <branch>
```

---

## Create a Branch and Worktree

```bash
git worktree add -b <new-branch> <directory>
```

---

## List Worktrees

```bash
git worktree list
```

---

## Remove a Worktree

```bash
git worktree remove <directory>
```

---

## Clean Stale Metadata

```bash
git worktree prune
```

---

## View Help

```bash
git worktree help
```

---

# Quick Mental Model

The easiest way to remember the difference:

```text
git switch
     ↓
One folder
     ↓
Change branches
```

Whereas:

```text
git worktree
     ↓
Multiple folders
     ↓
Multiple branches
     ↓
Work simultaneously
```

Another simple representation:

```text
             Git Repository
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
    main        feature       bugfix
       │           │           │
       ▼           ▼           ▼
   Folder A     Folder B     Folder C
```

---

# Key Takeaways

* A **Git Worktree** provides an additional working directory for the same repository.
* Multiple branches can be worked on simultaneously using separate directories.
* `git switch` changes branches inside the current directory.
* `git worktree` allows multiple directories to remain active at the same time.
* Worktrees are especially useful for urgent bug fixes, code reviews, parallel feature development, and long-running processes.
* Removing a worktree does **not** delete its branch.
* `git worktree prune` removes stale worktree metadata.
* A branch is normally checked out in only one worktree at a time.

---

# Practice Exercises

* [ ] Create a new branch and worktree using `git worktree add -b`.
* [ ] Create a second worktree for a bugfix.
* [ ] Use `git worktree list` to inspect all worktrees.
* [ ] Make changes independently in two worktrees.
* [ ] Commit changes from a worktree.
* [ ] Remove the worktree using `git worktree remove`.
* [ ] Check whether the branch still exists.
* [ ] Practice cleaning stale metadata using `git worktree prune`.

---

# What I Learned

After studying Git Worktrees, I learned:

* What a Git Worktree is.
* How Worktrees differ from `git switch`.
* How to create and manage multiple working directories.
* How to work on multiple branches simultaneously.
* How Worktrees help with urgent bug fixes and code reviews.
* How to remove worktrees without deleting branches.
* How to clean stale worktree metadata.

````
