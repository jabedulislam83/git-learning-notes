# Git Cherry-Pick

## Overview

Git provides several ways to integrate changes between branches.

So far, we have learned:

- Merge
- Rebase
- Squash
- Octopus

These approaches generally work with branch histories or multiple commits.

**Cherry-Pick** is different. It allows us to select a specific commit from one branch and apply its changes to another branch.

---

# What is Cherry-Pick?

The command is:

```bash
git cherry-pick
```

Cherry-Pick can be thought of as a **surgical Git operation**.

Instead of integrating an entire branch, we select only the specific commit we need.

---

# Cherry-Pick vs Merge

Suppose we have:

```text
main
A──B

feature
   \
    C──D──E
```

If we merge the feature branch:

```bash
git switch main
git merge feature
```

the feature branch's changes are integrated as a whole.

With Cherry-Pick, if we only need commit `D`:

```bash
git switch main
git cherry-pick <D-hash>
```

Only the changes introduced by `D` are applied to `main`.

---

# Easy Way to Remember

```text
Merge
→ Bring the branch's work together.

Rebase
→ Reorganize branch history.

Cherry-Pick
→ Bring only a specific commit.
```

---

# Cherry-Pick Creates a New Commit

This is an important concept.

Suppose the original commit is:

```text
D → abc123
```

When we run:

```bash
git cherry-pick abc123
```

Git applies the changes from that commit and creates a **new commit** on the target branch.

Example:

```text
Feature:

A──B──C──D


Main:

A──B────D'
```

Here:

```text
D  → Original Commit
D' → New Cherry-Picked Commit
```

The changes may be the same, but the commits are different.

Therefore, their commit hashes are different.

```text
Original:

abc123


Cherry-picked:

xyz789
```

---

# Why Does the Hash Change?

A Cherry-Pick does not move the original commit itself.

Instead, Git creates a new commit containing the selected changes.

Therefore:

```text
Original Commit
      │
      │ changes copied
      ▼
New Commit
```

The new commit receives a different hash.

---

# Cherry-Picking Multiple Commits

You can also Cherry-Pick multiple specific commits.

Example:

```bash
git cherry-pick abc123 def456 ghi789
```

This applies the changes from:

```text
abc123
def456
ghi789
```

to the current branch.

---

# When to Use Cherry-Pick?

Cherry-Pick is useful in several situations.

---

## 1. Specific Bug Fix

Suppose:

```text
feature

A──B──C──D
```

Commit `D` contains a critical bug fix.

However, commits `B` and `C` contain unfinished feature work.

You do not want to merge the entire branch.

Instead:

```bash
git cherry-pick <D-hash>
```

Only the bug fix is applied to the target branch.

---

# 2. Applying the Same Fix to Multiple Branches

Suppose we have:

```text
main
release-v1
release-v2
```

A bug is fixed in one branch.

The same fix is required in the release branches.

Instead of merging the entire branch, the specific bug-fix commit can be Cherry-Picked into each required branch.

Conceptually:

```text
Bug Fix Commit
      │
      ├──→ main
      ├──→ release-v1
      └──→ release-v2
```

---

# 3. Hotfix

Imagine a critical production bug has been fixed in one commit.

The fix needs to be applied quickly to a stable branch.

Cherry-Pick can be useful here because only the required fix needs to be transferred.

```text
Critical Fix
     │
     ▼
Cherry-Pick
     │
     ▼
Stable Branch
```

---

# 4. Selectively Applying a Feature Change

Suppose a feature branch contains five commits:

```text
A──B──C──D──E
```

You only want to test or use the changes from commit `C`.

Instead of integrating the whole feature:

```bash
git cherry-pick <C-hash>
```

Only the selected commit's changes are applied.

---

# 5. Recovering a Lost Commit

Sometimes a useful commit may no longer be part of the current branch history.

If the commit can still be identified by its hash, its changes may be applied to another branch using Cherry-Pick.

```bash
git cherry-pick <commit-hash>
```

---

# Cherry-Pick Example

Consider our **Flight Data Pipeline** project.

We have:

```text
main
A──B

data-validation
   \
    C──D──E
```

Suppose:

```text
D = Optimize data validation
```

We want only the optimization from `D`.

We do not want:

```text
C
E
```

---

# Performing the Cherry-Pick

First, switch to the target branch:

```bash
git switch main
```

Then:

```bash
git cherry-pick def456
```

Here:

```text
def456
```

is the hash of the commit we want.

---

# What Git Does

Original branch:

```text
C──D──E
   ↑
 def456
```

After Cherry-Pick:

```text
main

A──B──D'
```

The changes introduced by `D` have been applied to `main`.

---

# Important: `D` and `D'` Are Different Commits

```text
D
↓
Original Commit

D'
↓
Cherry-Picked New Commit
```

The changes may be equivalent, but the commit objects are different.

Therefore:

```text
D  → Original Hash
D' → New Hash
```

---

# Cherry-Pick Conflicts

Cherry-Pick does not always complete automatically.

A conflict can occur if the selected commit modifies content that has also been changed differently in the target branch.

Example:

```text
main
A──B──C

feature
   \
    D
```

Suppose both `C` and `D` modify the same part of a file differently.

Then:

```bash
git cherry-pick <D-hash>
```

may stop with a conflict.

---

# Resolving a Cherry-Pick Conflict

## Step 1 — Check the Status

Run:

```bash
git status
```

Git will show the conflicted files.

---

## Step 2 — Resolve the File

Open the conflicted file.

You may see markers such as:

```text
<<<<<<< HEAD
main version
=======
cherry-picked version
>>>>>>> def456
```

Choose the correct content and remove the conflict markers.

---

## Step 3 — Stage the Resolved File

```bash
git add <file>
```

---

## Step 4 — Continue the Cherry-Pick

```bash
git cherry-pick --continue
```

Git will continue the Cherry-Pick process.

---

# Canceling a Cherry-Pick

If you decide that you do not want to continue the current Cherry-Pick:

```bash
git cherry-pick --abort
```

This cancels the ongoing Cherry-Pick operation and attempts to return the branch to its previous state.

---

# Cherry-Pick Conflict Workflow

```text
git cherry-pick <commit>
        ↓
      CONFLICT
        ↓
nano filename
        ↓
Edit conflict
        ↓
Remove conflict markers
        ↓
Ctrl + O → Enter
        ↓
Ctrl + X
        ↓
git add .
        ↓
git cherry-pick --continue
```

To cancel:

```bash
git cherry-pick --abort
```

---

# When to Use Cherry-Pick

Cherry-Pick is particularly useful when:

### Specific isolated change is needed

```text
A──B──C──D──E

Only C is needed
```

Use:

```bash
git cherry-pick <C-hash>
```

---

### A production hotfix is needed

```text
Bug Fix
   │
   ▼
Cherry-Pick
   │
   ▼
Stable Branch
```

---

### The same fix is needed on multiple branches

```text
                Bug Fix
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
      main     release-v1  release-v2
```

---

### Only part of a feature is needed

Instead of merging the complete feature branch, select the required commit.

---

# Avoid Overusing Cherry-Pick

Cherry-Pick is powerful, but excessive use can create confusing history.

Suppose:

```text
feature
   D
```

The commit is Cherry-Picked into `main`:

```text
feature:
D

main:
D'
```

Now two different commits contain the same logical change.

```text
D  → Original
D' → Cherry-Picked
```

This can make the history more difficult to understand.

---

# When Merge or Rebase May Be Better

If you need to integrate a large feature containing many related commits, Cherry-Pick may not be the best choice.

For example:

```text
Feature
A──B──C──D──E
```

If the entire feature is needed, consider:

```bash
git merge feature
```

or, depending on the workflow:

```bash
git rebase main
```

Cherry-Pick is most useful when the goal is to select specific changes.

---

# Merge vs Rebase vs Cherry-Pick

| Aspect | Merge | Rebase | Cherry-Pick |
|--------|-------|--------|-------------|
| Works primarily with | Branches | Branch history | Specific commits |
| Can integrate many commits | Yes | Yes | Yes, if selected |
| Main purpose | Branch integration | History reorganization | Selective change transfer |
| History | Preserves branching | Rewrites history | Creates new commit |
| Best use | Feature integration | Clean history | Hotfix / isolated change |

---

# Real-World Example

Suppose:

```text
main
A──B

feature
   \
    C──D──E
```

## If the entire feature is required

```bash
git switch main
git merge feature
```

The complete feature is integrated.

---

## If the feature history needs reorganizing

```bash
git switch feature
git rebase main
```

The feature commits are recreated on top of `main`.

---

## If only `D` is required

```bash
git switch main
git cherry-pick <D-hash>
```

Result:

```text
main

A──B──D'
```

---

# Comparison at a Glance

```text
MERGE
│
└── Entire branch integration


REBASE
│
└── Reorganize branch history


CHERRY-PICK
│
└── Select a specific commit
```

---

# Common Beginner Mistakes

- Assuming Cherry-Pick moves the original commit.
- Forgetting that Cherry-Pick creates a new commit.
- Assuming the new commit will have the same hash.
- Cherry-Picking an entire feature when a merge would be more appropriate.
- Overusing Cherry-Pick and creating duplicate-looking history.
- Forgetting `git cherry-pick --continue` after resolving a conflict.
- Forgetting that an ongoing Cherry-Pick can be canceled with `--abort`.

---

# Best Practices

- Use Cherry-Pick for isolated changes.
- Use it carefully for production hotfixes.
- Use meaningful commit history when transferring fixes between branches.
- Avoid unnecessary Cherry-Picks of large feature histories.
- Check the commit graph after applying important Cherry-Picks.
- Resolve conflicts carefully before continuing.

---

# Summary

Git Cherry-Pick allows you to select a specific commit from one branch and apply its changes to another branch.

Unlike Merge, which generally integrates branch changes, Cherry-Pick focuses on an individual commit.

Cherry-Picking creates a new commit on the target branch, so the new commit has a different hash from the original.

It is particularly useful for:

- Isolated bug fixes
- Hotfixes
- Applying the same fix to multiple branches
- Selectively transferring feature changes
- Recovering useful commits

However, excessive Cherry-Picking can create duplicate-looking changes and make project history harder to understand.

---

# Key Takeaways

- `git cherry-pick` applies a specific commit to the current branch.
- It does not move the original commit.
- Cherry-Pick creates a new commit.
- The new commit has a different hash.
- Multiple commits can be Cherry-Picked.
- Conflicts can occur during Cherry-Pick.
- `git cherry-pick --continue` continues after conflict resolution.
- `git cherry-pick --abort` cancels an ongoing Cherry-Pick.
- Cherry-Pick is especially useful for isolated fixes and hotfixes.
- Overusing Cherry-Pick can make history more complicated.

---

# Practice Commands

## Cherry-Pick a Commit

```bash
git switch main

git cherry-pick <commit-hash>
```

## Cherry-Pick Multiple Commits

```bash
git cherry-pick <hash1> <hash2> <hash3>
```

## Check Status During a Conflict

```bash
git status
```

## Continue After Resolving a Conflict

```bash
git add <file>

git cherry-pick --continue
```

## Abort Cherry-Pick

```bash
git cherry-pick --abort
```

---

# Exercises

- [ ] Create two branches with different commits.
- [ ] Cherry-Pick one specific commit into another branch.
- [ ] Compare the original and Cherry-Picked commit hashes.
- [ ] Cherry-Pick multiple commits.
- [ ] Create a Cherry-Pick conflict and resolve it.
- [ ] Practice `git cherry-pick --continue`.
- [ ] Practice `git cherry-pick --abort`.
- [ ] Compare Cherry-Pick with Merge and Rebase.

---

# What I Learned

After studying this chapter, I learned:

- What Git Cherry-Pick is.
- How to apply a specific commit to another branch.
- Why Cherry-Pick creates a new commit.
- Why the Cherry-Picked commit has a different hash.
- How to Cherry-Pick multiple commits.
- How to resolve Cherry-Pick conflicts.
- When Cherry-Pick is useful for hotfixes and isolated changes.
- Why excessive Cherry-Picking can make history complicated.
- How Cherry-Pick differs from Merge and Rebase.