# Version History

## Overview

Every Git repository keeps a complete history of changes through commits. This history allows developers to understand how a project has evolved over time, review previous work, identify bugs, and restore earlier versions when needed.

Git provides several commands to inspect the project's history, making it easy to navigate through previous commits.

---

# What is Version History?

Version history is a chronological record of all commits made in a Git repository.

Each commit represents a snapshot of the project at a specific point in time.

The version history allows you to:

- Review previous work
- Understand project evolution
- Identify when changes were introduced
- Restore earlier versions
- Collaborate with confidence

---

# Viewing Commit History

The most common command for viewing commit history is:

```bash
git log
```

Example output:

```text
commit a7d23d1e5...
Author: Jabedul Islam
Date: July 17, 2026

Add Git Repository notes

commit 4f6c982...
Author: Jabedul Islam
Date: July 16, 2026

Add Terminal Basics notes
```

By default, Git displays the newest commit first.

---

# Viewing a Compact History

A shorter version of the commit history can be displayed using:

```bash
git log --oneline
```

Example:

```text
a7d23d1 Add Git Repository notes
4f6c982 Add Terminal Basics notes
91ab334 Initial repository structure
```

This format is useful when working with repositories that have many commits.

---

# Understanding Commit Hashes

Every commit has a unique identifier called a **commit hash**.

Example:

```text
91ab334f56f2d93e5c8...
```

Git uses this hash to uniquely identify each commit.

Even if two commits contain similar changes, their hashes will always be different.

---

# Viewing Commit Details

To inspect the latest commit:

```bash
git show
```

Example output includes:

- Commit hash
- Author
- Date
- Commit message
- Files changed
- Added and removed lines

You can also inspect a specific commit:

```bash
git show <commit-hash>
```

Example:

```bash
git show a7d23d1
```

---

# Understanding HEAD

`HEAD` is a special pointer that refers to the current commit you are working on.

For example:

```text
HEAD
│
▼
Latest Commit
```

After creating a new commit, `HEAD` automatically moves to that commit.

---

# Viewing the Latest Commit

Display only the most recent commit.

```bash
git log -1
```

Example:

```text
commit a7d23d1...

Add Git Repository notes
```

---

# Navigating Previous Commits

Git allows you to reference previous commits relative to `HEAD`.

Current commit:

```text
HEAD
```

Previous commit:

```text
HEAD~1
```

Two commits before:

```text
HEAD~2
```

Three commits before:

```text
HEAD~3
```

These references are useful for comparing versions and restoring previous states.

---

# Real-World Example

Suppose your repository contains these commits:

```text
Initial repository structure

Add README

Add Version Control notes

Add Terminal Basics notes

Add Git Repository notes
```

To see the history:

```bash
git log --oneline
```

Output:

```text
e58a23c Add Git Repository notes

f421bb3 Add Terminal Basics notes

a981ff2 Add Version Control notes

d223ad7 Add README

91ab334 Initial repository structure
```

To inspect the latest commit:

```bash
git show
```

---

# Best Practices

- Review commit history regularly.
- Write meaningful commit messages.
- Keep commits small and focused.
- Use `git log --oneline` for a cleaner overview.
- Use `git show` before reverting or comparing commits.

---

# Summary

Git maintains a complete history of every commit made in a repository. Commands such as `git log`, `git log --oneline`, and `git show` help developers inspect project history, understand previous changes, and manage software development more effectively.

Understanding version history is essential before learning how to compare versions or undo changes.

---

# Key Takeaways

- Git stores every commit permanently in the repository history.
- `git log` displays detailed commit history.
- `git log --oneline` provides a compact view.
- Every commit has a unique hash.
- `HEAD` points to the current commit.
- `git show` displays detailed information about a commit.

---

# Practice Commands

```bash
git log

git log --oneline

git log -1

git show

git show HEAD

git show HEAD~1
```

---

# Exercises

- [ ] View the complete commit history.
- [ ] Display the history using `git log --oneline`.
- [ ] Inspect the latest commit with `git show`.
- [ ] Find the latest commit hash.
- [ ] Identify which commit `HEAD` is pointing to.

---

# What I Learned

After studying this chapter, I learned:

- How Git stores project history.
- How to read commit history.
- The purpose of commit hashes.
- How the `HEAD` pointer works.
- How to inspect commits using `git show`.