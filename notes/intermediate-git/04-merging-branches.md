# Merging Branches

## Overview

One of Git's most important features is the ability to merge branches. After completing work on a feature branch, developers combine those changes with another branch, usually the **main** branch.

Merging allows independent development while keeping the project's official version up to date.

---

# What is a Merge?

A merge is the process of combining changes from one branch into another.

Instead of copying files manually, Git intelligently combines the commit histories of two branches.

In most projects, developers merge feature branches into the **main** branch after testing is complete.

---

# Why Do We Merge Branches?

Branches allow developers to work independently.

However, completed work eventually needs to become part of the official project.

Merging helps developers:

- Add completed features
- Apply bug fixes
- Keep the main branch updated
- Combine work from multiple developers

---

# Source and Destination Branch

Every merge involves two branches.

## Source Branch

The branch that contains the new changes.

Example:

```text
ai-assistant
```

---

## Destination Branch

The branch that will receive those changes.

Usually:

```text
main
```

Easy way to remember:

```text
Source
FROM
↓

Destination
TO
```

---

# Merge Workflow

Suppose your repository looks like this.

```text
main

A ─── B ─── C

             \
              \
               D ─── E

         ai-assistant
```

The **ai-assistant** branch contains the completed feature.

Now merge it into **main**.

---

# Step 1 — Switch to the Destination Branch

Always move to the destination branch first.

```bash
git switch main
```

Now Git knows where the merged changes should be placed.

---

# Step 2 — Merge the Source Branch

```bash
git merge ai-assistant
```

Git combines the commits from **ai-assistant** into **main**.

---

# Repository After Merge

```text
Before

main

A ─── B ─── C

             \
              \
               D ─── E

After

main

A ─── B ─── C ─── D ─── E
```

If no conflicts exist, Git performs the merge automatically.

---

# Understanding Fast-forward Merge

Fast-forward merge happens when the destination branch has not changed after the feature branch was created.

Example:

```text
Before

main

A ─── B

          \
           \
            C ─── D

        feature
```

The **main** branch has no additional commits.

After merging:

```text
main

A ─── B ─── C ─── D
```

Instead of creating a new merge commit, Git simply moves the **main** branch pointer forward.

This is called a **Fast-forward Merge**.

---

# When Does Fast-forward Merge Occur?

Fast-forward merge happens when:

- The destination branch has not received new commits.
- Only the feature branch contains additional commits.

In this situation, Git can safely move the branch pointer.

---

# Understanding Merge Output

After running:

```bash
git merge ai-assistant
```

Git may display:

```text
Updating 0b92e3f..a52f3de

Fast-forward

11 insertions(+)

create mode 100644 src/main.py
```

Let's understand each line.

---

## Updating

```text
Updating 0b92e3f..a52f3de
```

Git is updating the repository from one commit to another.

These commit IDs represent the parent commits involved in the merge.

---

## Fast-forward

```text
Fast-forward
```

Git moved the branch pointer without creating a new merge commit.

---

## Insertions

```text
11 insertions(+)
```

Git added 11 new lines.

If you see:

```text
2 deletions(-)
```

Git removed two lines.

---

## Create Mode

```text
create mode 100644 src/main.py
```

A new file was added during the merge.

---

# Parent Commits

Every merge is based on existing commits.

Example:

```text
Updating

0b92e3f

↓

a52f3de
```

Git compares these commits before combining the branches.

---

# Real-World Example

Suppose you are developing your portfolio website.

Main branch:

```text
Home

About

Projects
```

Feature branch:

```text
Home

About

Projects

Contact
```

Switch to the main branch.

```bash
git switch main
```

Merge the feature.

```bash
git merge contact-page
```

Your main branch now contains:

```text
Home

About

Projects

Contact
```

The Contact page is now part of the official website.

---

# Common Beginner Mistakes

- Merging while on the wrong branch.
- Forgetting to switch to the destination branch.
- Assuming every merge creates a merge commit.
- Merging unfinished work.

---

# Best Practices

- Test feature branches before merging.
- Switch to the destination branch first.
- Use meaningful branch names.
- Keep feature branches focused on one task.
- Delete feature branches after a successful merge.

---

# Summary

Merging combines changes from one branch into another.

The most common workflow is to switch to the destination branch and merge the completed feature branch.

When the destination branch has not changed, Git performs a fast-forward merge by moving the branch pointer forward.

Understanding merges is essential for collaborative software development.

---

# Key Takeaways

- A merge combines two branches.
- Every merge has a source and a destination branch.
- Always switch to the destination branch before merging.
- Fast-forward merge moves the branch pointer instead of creating a merge commit.
- Merge output explains what Git changed.

---

# Practice Commands

```bash
git switch main

git merge feature-login

git branch

git log --oneline
```

---

# Exercises

- [ ] Identify the source branch.
- [ ] Identify the destination branch.
- [ ] Merge a feature branch into main.
- [ ] Explain when a fast-forward merge occurs.
- [ ] Interpret a merge output message.

---

# What I Learned

After studying this chapter, I learned:

- What a Git merge is.
- The difference between source and destination branches.
- The correct merge workflow.
- How fast-forward merge works.
- How to understand Git merge output.