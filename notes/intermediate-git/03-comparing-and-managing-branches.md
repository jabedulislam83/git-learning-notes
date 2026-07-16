# Comparing and Managing Branches

## Overview

As a project grows, multiple branches are created for different features, bug fixes, and experiments. Git provides several commands to compare branches, rename them, and remove branches that are no longer needed.

These operations help keep repositories organized and make collaboration more efficient.

---

# Why Compare Branches?

Developers often need to answer questions like:

- What changed in another branch?
- Which files are different?
- Has a feature branch added new files?
- What will be merged into the main branch?

Git provides the `git diff` command to answer these questions.

---

# Comparing Two Branches

Use the following command:

```bash
git diff <branch1> <branch2>
```

Example:

```bash
git diff main summary-statistics
```

Git compares **branch1** with **branch2** and displays the differences.

---

# Understanding Diff Output

Suppose the **main** branch contains:

```text
README.md

Hello World
```

The **summary-statistics** branch contains:

```text
README.md

Hello World

Statistics Added
```

Running:

```bash
git diff main summary-statistics
```

Output:

```diff
+ Statistics Added
```

The `+` symbol means a new line has been added.

---

# Reading Git Diff Symbols

Git uses symbols to indicate changes.

| Symbol | Meaning |
|---------|---------|
| `+` | Added line |
| `-` | Removed line |
| Space | Unchanged line |

Example:

```diff
- Old Report
+ New Report
```

This indicates that the old line was replaced by a new one.

---

# Comparing Large Branches

Large projects may contain hundreds of changed files.

Git displays the output page by page.

Useful shortcuts:

| Key | Action |
|-----|--------|
| Space | Next page |
| b | Previous page |
| q | Quit |

These shortcuts work in Git's pager (`less`).

---

# Renaming a Branch

Sometimes a branch name no longer describes its purpose.

Suppose you created:

```text
feature_dev
```

Later you realize the branch is specifically for a chatbot.

Rename it.

```bash
git branch -m feature_dev chatbot
```

Now the branch becomes:

```text
main

chatbot
```

---

# Renaming the Current Branch

If you are already inside the branch you want to rename:

```bash
git branch -m chatbot
```

Replace **chatbot** with the new name.

Example:

```bash
git branch -m feature-login
```

---

# Checking Branches

Display all local branches.

```bash
git branch
```

Example:

```text
* main
  chatbot
```

The old branch name no longer appears.

---

# Deleting a Branch

After a feature has been merged into the main branch, the branch is usually no longer needed.

Delete it safely.

```bash
git branch -d chatbot
```

Example output:

```text
Deleted branch chatbot
```

Git removes the branch because it has already been merged.

---

# Force Deleting a Branch

Suppose the branch has **not** been merged.

Running:

```bash
git branch -d chatbot
```

may produce:

```text
error: The branch is not fully merged.
```

If you are certain you no longer need the branch:

```bash
git branch -D chatbot
```

The capital **D** forces Git to delete the branch.

Use this command carefully because unmerged work may be lost.

---

# Understanding -d vs -D

| Command | Purpose |
|----------|---------|
| `git branch -d` | Safely delete a merged branch |
| `git branch -D` | Force delete a branch even if it has not been merged |

In most situations, prefer `-d`.

---

# Real-World Example

Suppose your repository contains:

```text
main

feature-login

payment

dashboard
```

After completing and merging the **feature-login** branch:

Rename if necessary.

```bash
git branch -m feature-login login-feature
```

Delete the merged branch.

```bash
git branch -d login-feature
```

Your repository becomes cleaner and easier to maintain.

---

# Common Beginner Mistakes

- Comparing the wrong branches.
- Forgetting which branch is currently active.
- Force deleting a branch without checking its status.
- Using generic branch names.

---

# Best Practices

- Compare branches before merging.
- Use meaningful branch names.
- Delete merged branches regularly.
- Avoid using `-D` unless absolutely necessary.
- Keep your repository clean and organized.

---

# Summary

Git provides powerful tools for managing branches.

Developers can compare branches using `git diff`, rename branches using `git branch -m`, and safely remove completed branches using `git branch -d`.

These commands help maintain a clean and organized repository.

---

# Key Takeaways

- `git diff branch1 branch2` compares two branches.
- Git highlights added and removed lines.
- `git branch -m` renames a branch.
- `git branch -d` safely deletes a merged branch.
- `git branch -D` force deletes a branch.

---

# Practice Commands

```bash
git branch

git diff main feature-login

git branch -m feature-login login-feature

git branch -d login-feature

git branch -D login-feature
```

---

# Exercises

- [ ] Compare two branches.
- [ ] Rename a branch.
- [ ] Display all branches.
- [ ] Delete a merged branch.
- [ ] Explain the difference between `-d` and `-D`.

---

# What I Learned

After studying this chapter, I learned:

- How to compare branches.
- How Git displays differences.
- How to rename branches.
- How to safely delete branches.
- The difference between safe deletion and force deletion.