# Git Repository

## Overview

A Git repository is the foundation of every Git project. It is a directory that Git monitors and manages, allowing developers to track changes, maintain version history, and collaborate efficiently.

Every Git repository contains a hidden `.git` directory, which stores all the information Git needs to manage the project.

---

# What is a Git Repository?

A Git repository (or repo) is a project folder that has been initialized with Git.

It contains:

- Project files
- Commit history
- Branches
- Configuration files
- Metadata stored inside the hidden `.git` folder

Once a folder becomes a Git repository, Git starts tracking changes made to the files inside it.

---

# Types of Git Repositories

## Local Repository

A repository stored on your own computer.

Example:

```text
D:/Projects/git-learning-notes
```

This is where you write code, create commits, and manage your project locally.

---

## Remote Repository

A repository hosted on an online platform such as GitHub.

Example:

```text
https://github.com/username/git-learning-notes
```

A remote repository allows you to:

- Backup your work
- Share projects
- Collaborate with others
- Access your code from different devices

---

# Repository Structure

A typical Git repository looks like this:

```text
git-learning-notes/
│
├── README.md
├── notes/
├── assets/
└── .git/
```

The `.git` directory is created automatically after running `git init`.

---

# The .git Directory

The `.git` folder is the heart of a Git repository.

It stores:

- Commit history
- Branch information
- Configuration
- References
- Objects
- Tags

You should never modify the contents of the `.git` folder manually.

---

# Initializing a Repository

To create a new Git repository:

```bash
git init
```

Example:

```bash
mkdir git-learning-notes

cd git-learning-notes

git init
```

Output:

```text
Initialized empty Git repository...
```

Git now starts tracking this folder.

---

# Checking Repository Status

Use:

```bash
git status
```

Example output:

```text
On branch main

No commits yet

nothing to commit
```

The `git status` command displays:

- Current branch
- Staged files
- Modified files
- Untracked files

It is one of the most frequently used Git commands.

---

# Understanding Untracked Files

Suppose you create a new file:

```text
README.md
```

Before running `git add`, Git shows:

```text
Untracked files:

README.md
```

Git can see the file, but it is **not yet tracking it**.

---

# Tracking Files

Start tracking a file using:

```bash
git add README.md
```

Or track all files:

```bash
git add .
```

After staging, the file becomes ready to be committed.

---

# Repository Workflow

The basic Git workflow is:

```text
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Repository (Commit History)
```

This workflow is repeated throughout the life of a project.

---

# Real-World Example

Imagine you are building a portfolio website.

Step 1

```bash
mkdir portfolio
```

Step 2

```bash
cd portfolio
```

Step 3

```bash
git init
```

Step 4

Create:

```text
README.md
```

Step 5

```bash
git status
```

Git reports:

```text
Untracked files:

README.md
```

Step 6

```bash
git add README.md
```

The file is now staged and ready for a commit.

---

# Best Practices

- Initialize Git at the beginning of every project.
- Keep repositories organized.
- Write a meaningful README file.
- Check `git status` frequently.
- Avoid editing the `.git` directory manually.

---

# Summary

A Git repository is the central place where Git stores project history and tracks changes. Every repository contains a hidden `.git` directory that manages commits, branches, and other Git metadata.

Understanding repositories is essential before learning commits, branches, and collaboration.

---

# Key Takeaways

- A repository is a project managed by Git.
- Every repository contains a hidden `.git` directory.
- `git init` initializes a new repository.
- `git status` displays the current repository status.
- Files must be staged before they can be committed.

---

# Practice Commands

```bash
mkdir demo-project

cd demo-project

git init

git status

touch README.md

git status

git add README.md

git status
```

---

# Exercises

- [ ] Create a new project folder.
- [ ] Initialize a Git repository.
- [ ] Create a README.md file.
- [ ] Check the repository status.
- [ ] Stage the README.md file.

---

# What I Learned

After studying this chapter, I learned:

- What a Git repository is.
- The difference between local and remote repositories.
- The purpose of the hidden `.git` directory.
- How to initialize a repository.
- How Git tracks new files using `git status` and `git add`.