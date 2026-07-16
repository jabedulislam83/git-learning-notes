# Git Cheat Sheet

> A quick reference guide for the most commonly used Git commands learned from the **Introduction to Git** course.

> **Note:** This cheat sheet summarizes the Git commands covered in the **Introduction to Git** course. It is intended as a quick reference for practice and daily use. More advanced commands will be added as I complete the **Intermediate Git** course.
---

# Repository

Initialize a new Git repository.

```bash
git init
```

Check the current repository status.

```bash
git status
```

Display the installed Git version.

```bash
git --version
```

---

# Staging Changes

Stage a specific file.

```bash
git add filename
```

Example:

```bash
git add README.md
```

Stage all changes.

```bash
git add .
```

Remove a file from the staging area.

```bash
git restore --staged filename
```

Example:

```bash
git restore --staged README.md
```

---

# Creating Commits

Create a commit.

```bash
git commit -m "Commit message"
```

Example:

```bash
git commit -m "Add Version Control notes"
```

---

# Viewing Commit History

View detailed commit history.

```bash
git log
```

View a compact history.

```bash
git log --oneline
```

View the latest commit.

```bash
git log -1
```

---

# Viewing Commit Details

Show the latest commit.

```bash
git show
```

Show a specific commit.

```bash
git show <commit-hash>
```

Example:

```bash
git show a7d23d1
```

---

# Comparing Changes

Compare working directory changes.

```bash
git diff
```

Compare staged changes.

```bash
git diff --staged
```

Compare with the latest commit.

```bash
git diff HEAD
```

Compare the latest two commits.

```bash
git diff HEAD~1 HEAD
```

Compare a specific file.

```bash
git diff README.md
```

---

# Restoring Changes

Restore a modified file.

```bash
git restore README.md
```

Restore all modified files.

```bash
git restore .
```

Remove a staged file.

```bash
git restore --staged README.md
```

---

# Reverting Commits

Revert the latest commit.

```bash
git revert HEAD
```

Revert a specific commit.

```bash
git revert <commit-hash>
```

Example:

```bash
git revert a7d23d1
```

---

# Working with HEAD

Current commit.

```text
HEAD
```

Previous commit.

```text
HEAD~1
```

Two commits before.

```text
HEAD~2
```

---

# Useful Terminal Commands

Show current directory.

```bash
pwd
```

List files.

```bash
ls
```

List all files, including hidden files.

```bash
ls -a
```

Change directory.

```bash
cd folder-name
```

Go back one directory.

```bash
cd ..
```

Create a directory.

```bash
mkdir folder-name
```

Create an empty file.

```bash
touch README.md
```

---

# Basic Git Workflow

```text
Create Project
      │
      ▼
git init
      │
      ▼
Modify Files
      │
      ▼
git status
      │
      ▼
git add .
      │
      ▼
git commit -m "message"
      │
      ▼
git push
```

---

# Most Frequently Used Commands

```bash
git status

git add .

git commit -m "message"

git log --oneline

git show

git diff

git restore filename

git restore --staged filename

git revert HEAD
```

---

# Best Practices

- Commit frequently.
- Write meaningful commit messages.
- Review changes before committing.
- Keep commits focused on a single task.
- Push your work regularly to GitHub.
- Never edit the `.git` directory manually.

---

# Quick Tips

| Task | Command |
|------|---------|
| Initialize a repository | `git init` |
| Check repository status | `git status` |
| Stage all files | `git add .` |
| Create a commit | `git commit -m "message"` |
| View history | `git log` |
| Compact history | `git log --oneline` |
| View commit details | `git show` |
| Compare changes | `git diff` |
| Restore a file | `git restore file` |
| Unstage a file | `git restore --staged file` |
| Revert a commit | `git revert HEAD` |

---

# Summary

This cheat sheet provides a quick reference to the essential Git commands introduced in the **Introduction to Git** course. Keep it nearby while practicing Git to improve your workflow and build confidence with version control.