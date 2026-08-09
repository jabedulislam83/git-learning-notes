# Understanding Merge Types

## Overview

Merging is one of Git's most important features. It allows developers to combine changes from different branches into a single branch.

While basic Git introduces the `git merge` command, professional software development requires a deeper understanding of **merge strategies**. Different situations require different merge approaches, and choosing the correct strategy helps maintain a clean and understandable project history.

In this chapter, we explore the two most common merge strategies:

- Fast-forward Merge
- Recursive (Merge Commit) Merge

---

# What is `git merge`?

The `git merge` command combines changes from one branch into another.

For example:

```bash
git switch main

git merge feature-login
```

Git takes the commits from the **feature-login** branch and integrates them into the **main** branch.

---

# How Git Performs a Merge

Before merging, Git analyzes both branches.

It:

1. Finds the latest commit on each branch.
2. Searches for their common ancestor (base commit).
3. Selects the most appropriate merge strategy.
4. Combines the changes.

Illustration:

```text
A
│
B
├────── main
│
└────── feature
```

Commit **B** is the common base commit from which both branches diverged.

---

# Merge Strategies

A merge strategy defines **how Git combines two branches**.

Git automatically selects the most suitable strategy in most situations.

However, developers can override the default behavior using merge options.

Common merge strategies include:

- Fast-forward Merge
- Merge Commit (`--no-ff`)

---

# Fast-forward Merge

A fast-forward merge occurs when the target branch has not changed since the feature branch was created.

Example before merging:

```text
A──B──C (main)
        \
         D──E (feature)
```

Since the **main** branch has no additional commits, Git simply moves the branch pointer forward.

After merging:

```text
A──B──C──D──E (main)
```

No new merge commit is created.

---

# Characteristics of Fast-forward Merge

- Creates a clean, linear history.
- Does not create a merge commit.
- Moves the branch pointer forward.
- Best suited for short-lived feature branches.
- Makes project history simpler.

---

# Limitation of Fast-forward Merge

Although the history looks clean, the feature branch disappears from the commit graph.

Later, it becomes difficult to identify which commits originally belonged to that feature branch.

For large projects, this lack of historical context can become problematic.

---

# Forcing a Fast-forward Merge

To allow merging only when a fast-forward merge is possible:

```bash
git merge --ff-only feature
```

If Git cannot perform a fast-forward merge, the merge is aborted.

---

# Merge Commit (Recursive Merge)

When both branches contain new commits, Git cannot simply move a branch pointer.

Instead, Git creates a new merge commit that combines both histories.

Example before merging:

```text
A──B──E (main)
     \
      C──D (feature)
```

After merging:

```text
A──B──E────M
     \    /
      C──D
```

Here:

- **M** is the merge commit.
- It has two parent commits.

---

# Why Use a Merge Commit?

A merge commit preserves the complete development history.

Developers can clearly see:

- When the feature branch started.
- Which commits belong to the feature.
- When it was merged into the main branch.

This is especially useful for:

- Large projects
- Long-running feature branches
- Team collaboration

---

# Forcing a Merge Commit

Even if a fast-forward merge is possible, you can force Git to create a merge commit.

```bash
git merge --no-ff feature
```

This preserves the branch structure in the commit history.

---

# Fast-forward vs Merge Commit

| Feature | Fast-forward Merge | Merge Commit (`--no-ff`) |
|----------|--------------------|--------------------------|
| Creates new merge commit | ❌ | ✅ |
| Linear history | ✅ | ❌ |
| Preserves branch history | ❌ | ✅ |
| Best for | Small features | Large or long-lived features |
| Command | `--ff-only` | `--no-ff` |

---

# Real-World Example

Suppose you are developing a **Retail Intelligence Platform**.

You create a small branch to fix a typo in the README file.

```text
feature-readme
```

Since no one updates the **main** branch, Git performs a fast-forward merge.

---

Now imagine your team spends two weeks building a dashboard feature.

Meanwhile, the **main** branch receives several updates from other developers.

When the dashboard is complete, Git creates a merge commit to combine both histories while preserving the complete development timeline.

---

# Common Beginner Mistakes

- Assuming every merge creates a merge commit.
- Forgetting that fast-forward merges hide branch history.
- Using `--ff-only` without understanding its limitations.
- Confusing merge commits with ordinary commits.

---

# Best Practices

- Use fast-forward merges for small, simple branches.
- Use merge commits for significant features.
- Keep feature branches focused on a single task.
- Pull the latest changes before merging.
- Review changes before completing a merge.

---

# Summary

Git provides multiple merge strategies depending on the relationship between branches.

Fast-forward merges create a clean linear history without additional commits, while merge commits preserve the complete branch history by creating a dedicated merge commit.

Understanding these strategies helps developers maintain organized and meaningful project histories.

---

# Key Takeaways

- Git automatically selects an appropriate merge strategy.
- Fast-forward merges simply move the branch pointer.
- Merge commits preserve branch history.
- `--ff-only` allows only fast-forward merges.
- `--no-ff` forces the creation of a merge commit.
- Choosing the correct strategy depends on the project's workflow.

---

# Practice Commands

```bash
git merge feature

git merge --ff-only feature

git merge --no-ff 

git merge etl-update --no-ff --no-edit
```

---

# Exercises

- [ ] Explain the difference between fast-forward and merge commit merges.
- [ ] Create a feature branch and merge it using a fast-forward merge.
- [ ] Force a merge commit using `--no-ff`.
- [ ] Identify the common base commit in a merge diagram.
- [ ] Describe when a merge commit is preferable.

---

# What I Learned

After studying this chapter, I learned:

- How Git determines a merge strategy.
- The difference between fast-forward merges and merge commits.
- When each merge strategy should be used.
- How to force specific merge behaviors using Git options.
- Why preserving project history is important in collaborative development.