# Comparing Versions

## Overview

As a project evolves, files are continuously modified. Git provides powerful tools to compare different versions of files and commits, helping developers understand what has changed before creating a commit or reviewing previous work.

The primary command used for comparison is `git diff`.

---

# Why Compare Versions?

Comparing versions helps developers:

- Review changes before committing
- Identify mistakes
- Debug problems
- Verify modifications
- Understand project evolution

Instead of guessing what changed, Git clearly highlights every difference.

---

# Understanding File States

Before learning `git diff`, it is important to understand Git's three working areas.

```text
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Repository
```

- **Working Directory** → Files you are currently editing.
- **Staging Area** → Files prepared for the next commit.
- **Repository** → Files already committed.

Git compares these areas to display differences.

---

# Comparing Working Directory Changes

To compare the current working directory with the staging area:

```bash
git diff
```

Example:

Suppose your README.md originally contains:

```text
Git Learning Notes
```

You modify it to:

```text
Git Learning Notes

Introduction to Git
```

Running:

```bash
git diff
```

Output:

```diff
+ Introduction to Git
```

The `+` symbol indicates a newly added line.

---

# Comparing Staged Changes

After staging a file:

```bash
git add README.md
```

Compare staged changes with the last commit:

```bash
git diff --staged
```

or

```bash
git diff --cached
```

This shows what will be included in the next commit.

---

# Comparing with the Latest Commit

Compare your current work with the latest commit.

```bash
git diff HEAD
```

This command displays all changes since the most recent commit.

---

# Comparing Previous Commits

Git allows you to compare different commits.

Example:

```bash
git diff HEAD~1 HEAD
```

This compares:

- Previous commit
- Current commit

You can also compare any two commits using their hashes.

```bash
git diff abc123 def456
```

---

# Comparing a Specific File

Compare changes in a single file.

```bash
git diff README.md
```

This keeps the output focused on one file instead of the entire project.

---

# Understanding Diff Output

Example:

```diff
- Old Title
+ New Title
```

Symbols:

| Symbol | Meaning |
|--------|---------|
| `+` | Added line |
| `-` | Removed line |
| Space | Unchanged line |

Git highlights exactly what has changed between versions.

---

# Real-World Example

Suppose you update your project documentation.

Before editing:

```text
README.md
```

After editing:

```text
README.md

Added Installation Guide
```

Run:

```bash
git diff
```

Review the changes.

Then stage the file:

```bash
git add README.md
```

Verify staged changes:

```bash
git diff --staged
```

Finally commit:

```bash
git commit -m "Update README"
```

---

# Common Beginner Mistakes

- Forgetting to review changes before committing.
- Confusing `git diff` with `git log`.
- Comparing the wrong versions.
- Ignoring staged changes.

---

# Best Practices

- Run `git diff` before every commit.
- Review staged changes using `git diff --staged`.
- Compare commits when debugging.
- Keep commits small for easier comparison.

---

# Summary

Git makes it easy to compare different versions of files and commits. The `git diff` family of commands helps developers review modifications, detect mistakes, and understand project changes before committing them.

Learning how to compare versions is essential for writing high-quality software and maintaining clean project history.

---

# Key Takeaways

- `git diff` compares working directory changes.
- `git diff --staged` compares staged changes.
- `git diff HEAD` compares with the latest commit.
- Git clearly highlights added and removed lines.
- Reviewing differences before committing is a good habit.

---

# Practice Commands

```bash
git diff

git diff README.md

git diff --staged

git diff HEAD

git diff HEAD~1 HEAD
```

---

# Exercises

- [ ] Modify a file and run `git diff`.
- [ ] Stage the file and run `git diff --staged`.
- [ ] Compare the latest commit with the previous commit.
- [ ] Identify added and removed lines.
- [ ] Commit the reviewed changes.

---

# What I Learned

After studying this chapter, I learned:

- Why comparing versions is important.
- How to use the `git diff` command.
- The difference between working directory and staging area.
- How to compare staged changes.
- How Git highlights additions and deletions.