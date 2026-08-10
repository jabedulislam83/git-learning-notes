# Git Rebase

## Overview

Git provides more than one way to integrate changes between branches. Besides merging, **rebasing** is another approach for integrating branch changes.

Rebase is especially useful when you want to reorganize commit history and create a clean, linear commit graph.

In this chapter, we will learn:

- What Git Rebase is
- How rebasing works
- How to perform a rebase
- Interactive Rebase
- `pick` and `fixup`
- Merge vs Rebase
- When to use Merge or Rebase
- Why rebasing shared branches can be risky

---

# What is Git Rebase?

Git Rebase is another way to integrate changes between branches.

```text
Branch Integration
       │
       ├── Merge
       │
       └── Rebase
```

While merging combines two development histories, rebasing reorganizes a branch's commits to create a cleaner and more linear history.

---

# Why Use Rebase?

The main purpose of rebasing is to reorganize commit history.

Consider:

```text
A──B──C        main
     \
      D──E     feature
```

After rebasing:

```text
A──B──C──D'──E'    feature
```

The feature commits are placed after the latest commit from `main`.

This produces a linear history.

---

# Rebase Example

Suppose we have two branches:

```text
main
data-cleanup
```

Their history looks like:

```text
A──B──C        main
     \
      D──E     data-cleanup
```

Here:

- `A` → Common base
- `B`, `C` → New commits on `main`
- `D`, `E` → Feature commits

We want to update `data-cleanup` with the latest `main` history.

---

# Performing a Rebase

First, switch to the feature branch:

```bash
git switch data-cleanup
```

Then run:

```bash
git rebase main
```

---

# What Happens During Rebase?

Git performs several conceptual steps.

## Step 1 — Find the Common Base

Git identifies the common ancestor of the two branches.

```text
A──B──C        main
     \
      D──E     data-cleanup
```

Here:

```text
A = Common Base
B,C = main commits
D,E = Feature commits
```

---

## Step 2 — Temporarily Set Aside Feature Commits

Git temporarily removes the feature branch's commits from the branch being rebased.

Conceptually:

```text
A──B──C        main

D──E           Feature commits
```

These commits will be replayed later.

---

## Step 3 — Replay the Feature Commits

Git places the feature commits after the latest commit of `main`.

```text
A──B──C──D'──E'
             ↑
        data-cleanup
```

The result is a linear history.

---

# Why `D` Becomes `D'`

This is an important concept.

During a rebase, Git does not simply move the original commits.

Instead, it creates new commits containing the same changes.

For example:

Before:

```text
D → abc123
E → def456
```

After:

```text
D' → 789xyz
E' → 456pqr
```

Because the commits are recreated, their commit hashes change.

Therefore:

> Rebase can change the commit hashes of the commits being rebased.

---

# What Happens to `main`?

Rebasing the feature branch does not move the `main` branch.

After the rebase:

```text
main → C
```

while:

```text
data-cleanup → E'
```

The updated history is:

```text
A──B──C──D'──E'
             ↑
        data-cleanup
```

---

# Handling Rebase Conflicts

A rebase can encounter conflicts.

When this happens, Git pauses the rebase.

After resolving the conflicted file, stage it:

```bash
git add <file>
```

Then continue:

```bash
git rebase --continue
```

---

# Aborting a Rebase

If you decide that you do not want to continue the rebase, you can cancel it:

```bash
git rebase --abort
```

Git attempts to return the branch to its state before the rebase started.

---

# Interactive Rebase

Git also provides **Interactive Rebase**.

The command is:

```bash
git rebase -i
```

The `-i` option means:

```text
interactive
```

Interactive Rebase allows you to modify the history of multiple commits.

You can use it to:

- Change commit messages
- Combine commits
- Remove commits
- Reorder commits

---

# Starting an Interactive Rebase

Example:

```bash
git rebase -i HEAD~3
```

This opens an interactive editor for the selected commits.

---

# Understanding `HEAD~3`

Suppose the history is:

```text
A
│
B
│
C
│
D ← HEAD
```

Then:

```text
HEAD    = D
HEAD~1  = C
HEAD~2  = B
HEAD~3  = A
```

Therefore:

```bash
git rebase -i HEAD~3
```

tells Git to interactively rework the selected recent commits.

---

# Interactive Rebase Example

Suppose the `data-validation` branch contains:

```text
A
│
B  Add data validation
│
C  Fix validation bug
│
D  Optimize performance
```

We want to combine the work into a cleaner history.

Run:

```bash
git rebase -i HEAD~3
```

The editor may show:

```text
pick abc123 Add data validation
pick def456 Fix validation bug
pick ghi789 Optimize performance
```

Each line represents a commit and an action.

---

# `pick`

`pick` means:

> Keep the commit as it is.

Example:

```text
pick abc123 Add data validation
```

The commit remains in the history.

---

# `fixup`

`fixup` combines a commit with the previous commit and discards the fixup commit's separate message.

For example:

```text
pick   Add data validation
fixup  Fix validation bug
fixup  Optimize performance
```

The result is effectively one commit:

```text
Add data validation
```

The changes from all three commits are combined into that commit.

---

# Interactive Rebase Result

Before:

```text
B  Add data validation
C  Fix validation bug
D  Optimize performance
```

After:

```text
B'  Add data validation
```

The resulting commit contains the combined changes.

The commit hash may change because the history has been rewritten.

---

# Why Clean Up Commit History?

During development, it is common to create small temporary commits:

```text
fix typo
fix typo again
fix bug
fix bug again
test
test again
final fix
```

These commits may be useful during development but can make the final project history difficult to understand.

Interactive Rebase can help turn them into meaningful commits such as:

```text
Add data validation
Add flight data cleanup
Improve ETL performance
```

This makes the project history easier to read.

---

# Merge vs Rebase

Consider:

```text
A──B──C        main
     \
      D──E     data-cleanup
```

---

## Merge

With a merge:

```text
A──B──C────M
     \     /
      D──E
```

`M` is a merge commit.

The branching structure remains visible.

---

## Rebase

With a rebase:

```text
A──B──C──D'──E'
```

The history becomes linear.

---

# Main Difference

## Merge

> Merge combines separate development histories.

```text
Branch A
     \
      ── Merge ──► Combined History
     /
Branch B
```

The original branching structure remains visible.

---

## Rebase

> Rebase reorganizes the feature branch's commits on top of another branch.

```text
Original Feature
      │
      ▼
Rebase
      │
      ▼
Linear History
```

---

# Merge vs Rebase Comparison

| Aspect | Merge | Rebase |
|--------|-------|--------|
| History | Preserves existing history | Rewrites history |
| History shape | Branching may remain | Usually linear |
| Merge commit | May be created | Usually not created |
| Commit hashes | Usually preserved | Rebased hashes change |
| Parallel development | Clearly visible | Less explicit |
| Shared branch | Generally safer | Riskier |
| History cleanup | Less suitable | Very useful |

---

# When to Use Merge

Use `merge` when:

- You want to preserve development history.
- Parallel development context is important.
- You are integrating completed feature work.
- You are working with a shared/public branch.

Example:

```bash
git switch main
git merge data-cleanup
```

---

# When to Use Rebase

Rebase can be useful when:

## Updating a Feature Branch

Bring a feature branch up to date with the latest `main`:

```bash
git switch data-cleanup
git rebase main
```

---

## Cleaning Up Local History

Suppose your feature branch contains:

```text
fix
fix again
test
fix
final fix
```

Interactive Rebase can combine or reorganize these commits before the branch is integrated.

For example:

```text
Add data cleanup feature
```

---

# Important Rebase Warning

> **Rebase rewrites history.**

Because Git recreates commits during a rebase, commit hashes can change.

This is why rebasing shared branches requires caution.

---

# Why Can Rebase Be Risky on Shared Branches?

Suppose multiple developers are working from the same shared branch.

```text
GitHub
   │
   ▼
shared branch
```

If one developer rewrites its history using rebase, other developers may have a different version of that history locally.

This can lead to:

- Confusing synchronization
- Conflicting histories
- Push problems
- Duplicate-looking commits

Therefore, avoid rebasing shared/public history unless the team explicitly agrees on the workflow.

---

# Golden Rule

A useful rule is:

```text
Private Feature Branch
        │
        ▼
      Rebase
        ✅
```

For example:

```text
data-cleanup
```

If you are the only person working on it, rebasing is generally easier to manage.

For a shared/public branch:

```text
main
```

be cautious about rewriting history.

```text
Shared/Public Branch
        │
        ▼
      Rebase
        ⚠️
```

---

# Complete Rebase Workflow

Before:

```text
                 main
                  │
A──B──C───────────┘
     \
      D──E
      │
data-cleanup
```

Run:

```bash
git switch data-cleanup

git rebase main
```

After:

```text
A──B──C──D'──E'
             │
       data-cleanup
```

The feature commits have been recreated on top of `main`.

---

# Rebase Conflict Workflow

If a conflict occurs:

```text
git rebase main
        │
        ▼
    Conflict
        │
        ▼
Resolve File
        │
        ▼
git add <file>
        │
        ▼
git rebase --continue
```

To cancel:

```bash
git rebase --abort
```

---

# Interactive Rebase Workflow

```bash
git rebase -i HEAD~3
```

Then choose actions such as:

```text
pick
fixup
```

Example:

```text
pick   Add data validation
fixup  Fix validation bug
fixup  Optimize performance
```

Result:

```text
Add data validation
```

---

# Common Beginner Mistakes

- Rebasing a shared branch without understanding the consequences.
- Forgetting that rebase changes commit hashes.
- Assuming rebase is the same as merge.
- Forgetting `git rebase --continue` after resolving a conflict.
- Using `git rebase --abort` without understanding that it cancels the current rebase.
- Selecting the wrong range for interactive rebase.

---

# Best Practices

- Prefer rebase on private feature branches when you need a clean history.
- Avoid rewriting history that other developers are already using.
- Resolve conflicts carefully during rebase.
- Review the commit history after an interactive rebase.
- Use meaningful final commit messages.
- Understand the history impact before choosing rebase.

---

# Summary

Git Rebase is an alternative to merge for integrating branch changes.

Instead of creating a branching history, rebase recreates the feature commits on top of another branch, producing a cleaner and more linear history.

Interactive Rebase provides additional control over commit history. It allows developers to edit, reorder, combine, or remove commits.

However, because rebase rewrites history and can change commit hashes, it should be used carefully on shared or public branches.

---

# Key Takeaways

- `git rebase` reorganizes branch history.
- Rebase can create a clean, linear commit graph.
- Rebased commits receive new commit hashes.
- `git rebase -i` provides interactive history editing.
- `pick` keeps a commit as it is.
- `fixup` combines a commit with the previous commit and removes its separate message.
- `git rebase --continue` continues a paused rebase.
- `git rebase --abort` cancels the current rebase.
- Avoid rewriting shared/public history without coordination.

---

# Practice Commands

## Basic Rebase

```bash
git switch data-cleanup

git rebase main
```

## Interactive Rebase

```bash
git rebase -i HEAD~3
```

## Continue After Conflict Resolution

```bash
git add <file>

git rebase --continue
```

## Abort Rebase

```bash
git rebase --abort
```

---

# Exercises

- [ ] Create a feature branch from `main`.
- [ ] Add commits to both `main` and the feature branch.
- [ ] Rebase the feature branch onto `main`.
- [ ] Compare the commit graph before and after rebasing.
- [ ] Perform an interactive rebase using `HEAD~3`.
- [ ] Use `fixup` to combine multiple commits.
- [ ] Create and resolve a rebase conflict.
- [ ] Practice `git rebase --abort`.

---

# What I Learned

After studying this chapter, I learned:

- What Git Rebase is.
- How rebase differs from merge.
- How rebase creates a linear history.
- Why rebased commit hashes change.
- How to resolve rebase conflicts.
- How Interactive Rebase works.
- How `pick` and `fixup` can be used to clean up commits.
- Why rebasing shared branches can be risky.