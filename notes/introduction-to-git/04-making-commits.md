# Making Commits

## Overview

A commit is one of the most important concepts in Git. It represents a snapshot of your project at a specific point in time. Each commit records the changes you have made, allowing you to review project history, restore previous versions, and collaborate effectively.

Think of a commit as saving your progress in a video game. Every save point allows you to return to that exact state whenever necessary.

---

# What is a Commit?

A commit is a permanent record of changes made to your project.

Each commit contains:

- A unique commit ID (SHA hash)
- The author information
- The date and time
- A commit message
- The changes included in that commit

Once a commit is created, it becomes part of the project's history.

---

# Why Are Commits Important?

Commits help developers:

- Save project progress
- Track changes over time
- Restore previous versions
- Understand what changed and why
- Collaborate with team members

Without commits, managing software projects would be difficult and error-prone.

---

# The Git Commit Workflow

Before creating a commit, changes move through three stages.

```text
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Commit History
```

1. Modify files.
2. Stage the changes.
3. Create a commit.

---

# Checking Repository Status

Before committing, always check the repository status.

```bash
git status
```

Example:

```text
Changes to be committed:

new file: README.md
```

This tells you which files are ready to be committed.

---

# Staging Files

Stage a specific file.

```bash
git add README.md
```

Stage all changes.

```bash
git add .
```

Only staged files are included in the next commit.

---

# Creating a Commit

Create a commit using:

```bash
git commit -m "Initial commit"
```

Example:

```bash
git commit -m "Add Version Control notes"
```

Git records the current state of the staged files.

---

# Writing Good Commit Messages

A commit message should clearly describe the purpose of the changes.

Good examples:

```text
Add Version Control notes

Fix navigation bar alignment

Update README

Improve project documentation
```

Avoid messages like:

```text
Update

Changes

Test

abc
```

A meaningful message makes the project history easier to understand.

---

# Viewing Commit History

Display all commits.

```bash
git log
```

Display a shorter version.

```bash
git log --oneline
```

Example:

```text
a7d23d1 Add Git Repository notes
4f6c982 Add Terminal Basics notes
91ab334 Initial repository structure
```

---

# Understanding Commit IDs

Every commit has a unique SHA-1 hash.

Example:

```text
91ab334f56f2e4d3...
```

Git uses this ID to identify each commit.

Even if two commits have similar content, their commit IDs are always different.

---

# Real-World Example

Suppose you are writing documentation for a Git repository.

First, create a new file.

```text
04-making-commits.md
```

Check the status.

```bash
git status
```

Stage the file.

```bash
git add notes/introduction-to-git/04-making-commits.md
```

Create the commit.

```bash
git commit -m "Add Making Commits notes"
```

Push the commit.

```bash
git push
```

Your work is now safely stored in the local and remote repositories.

---

# Best Practices

- Commit frequently.
- Keep each commit focused on one task.
- Write meaningful commit messages.
- Review changes before committing.
- Push commits regularly to GitHub.

---

# Summary

A commit is a snapshot of your project that records staged changes. Commits help developers track project history, collaborate with others, and safely manage software development.

Learning to create clear and meaningful commits is one of the most valuable Git skills.

---

# Key Takeaways

- A commit records a snapshot of your project.
- Only staged files are committed.
- Every commit has a unique SHA hash.
- Good commit messages improve project history.
- Commit often and keep commits focused.

---

# Practice Commands

```bash
git status

git add .

git commit -m "Add Making Commits notes"

git log

git log --oneline
```

---

# Exercises

- [ ] Create a new Markdown file.
- [ ] Stage the file.
- [ ] Commit the changes.
- [ ] View the commit history.
- [ ] Read the latest commit using `git log`.

---

# What I Learned

After studying this chapter, I learned:

- What a Git commit is.
- The purpose of the staging area.
- How to create commits.
- How to write meaningful commit messages.
- How to view the commit history.