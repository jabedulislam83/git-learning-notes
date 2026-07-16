# Revert and Restore

## Overview

Mistakes are a natural part of software development. You may accidentally modify a file, stage the wrong changes, or commit something that should not have been committed.

Git provides several commands to safely recover from these situations. Two of the most commonly used commands are `git restore` and `git revert`.

Although both commands help undo changes, they serve different purposes.

---

# Why Undo Changes?

Undoing changes helps developers:

- Recover from mistakes
- Remove unwanted modifications
- Restore previous versions
- Keep project history clean
- Fix errors without losing important work

---

# Understanding Git States

Before using undo commands, remember Git's three working areas.

```text
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Repository
```

Different commands affect different areas.

---

# Restoring Changes in the Working Directory

Suppose you accidentally modify a file.

```
README.md
```

You decide to discard those changes.

Run:

```bash
git restore README.md
```

Git restores the file to its last committed version.

**Important:** Any uncommitted changes in that file will be permanently lost.

---

# Restoring All Modified Files

Restore every modified file.

```bash
git restore .
```

This command discards all uncommitted changes in the current directory.

Use it carefully.

---

# Unstaging Files

Sometimes you stage a file by mistake.

```bash
git add README.md
```

To remove it from the staging area:

```bash
git restore --staged README.md
```

The file remains unchanged.

Only the staging operation is undone.

---

# Reverting a Commit

Suppose you already committed your changes.

Instead of deleting history, Git creates a new commit that reverses the previous one.

```bash
git revert HEAD
```

Example:

```
Commit A

↓

Commit B

↓

git revert HEAD

↓

Commit C (undoes Commit B)
```

This keeps the repository history safe and is recommended for shared repositories.

---

# Reverting a Specific Commit

Use a commit hash.

```bash
git revert a7d23d1
```

Git creates a new commit that reverses the selected commit.

---

# Restore vs Revert

| Feature | git restore | git revert |
|----------|-------------|------------|
| Affects uncommitted changes | ✅ Yes | ❌ No |
| Affects committed history | ❌ No | ✅ Yes |
| Creates a new commit | ❌ No | ✅ Yes |
| Safe for shared repositories | — | ✅ Yes |

---

# What About git checkout?

Older Git tutorials often use:

```bash
git checkout
```

For example:

```bash
git checkout README.md
```

Modern Git recommends using:

```bash
git restore README.md
```

because it clearly explains the intention of the command.

`git checkout` is still supported, but `git restore` is easier to understand for beginners.

---

# Real-World Example

Suppose your repository contains:

```
README.md

notes/

assets/
```

You accidentally delete several lines from `README.md`.

Instead of manually rewriting them:

```bash
git restore README.md
```

Git restores the previous version immediately.

---

Another example:

You accidentally stage the wrong file.

```bash
git add README.md
```

Undo staging:

```bash
git restore --staged README.md
```

---

Another example:

You pushed a commit that introduced a bug.

Instead of deleting history:

```bash
git revert HEAD
```

Git creates a new commit that safely reverses the changes.

---

# Common Beginner Mistakes

- Using `git restore` after important work without checking changes.
- Confusing `git revert` with deleting commits.
- Forgetting that `git revert` creates a new commit.
- Accidentally restoring the wrong file.

---

# Best Practices

- Review changes before restoring them.
- Use `git restore --staged` when you stage the wrong files.
- Use `git revert` for undoing commits in shared repositories.
- Avoid deleting Git history unnecessarily.
- Read command output carefully before confirming changes.

---

# Summary

Git provides powerful recovery tools that allow developers to safely undo mistakes.

`git restore` is used for recovering uncommitted changes, while `git revert` creates a new commit that reverses a previous commit without removing project history.

Understanding these commands helps developers work confidently without worrying about making mistakes.

---

# Key Takeaways

- `git restore` discards uncommitted changes.
- `git restore --staged` removes files from the staging area.
- `git revert` safely reverses committed changes.
- `git revert` never deletes project history.
- `git checkout` is still available, but `git restore` is the preferred modern command.

---

# Practice Commands

```bash
git restore README.md

git restore .

git restore --staged README.md

git revert HEAD

git revert <commit-hash>
```

---

# Exercises

- [ ] Modify a file and restore it.
- [ ] Stage a file and remove it from the staging area.
- [ ] Review the latest commit.
- [ ] Revert the latest commit.
- [ ] Compare the repository before and after reverting.

---

# What I Learned

After studying this chapter, I learned:

- How to discard uncommitted changes.
- How to unstage files safely.
- The difference between restore and revert.
- When to use `git revert` instead of deleting history.
- How Git helps recover from mistakes while preserving project history.