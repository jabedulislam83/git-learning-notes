# Merge Conflicts

## Overview

A merge conflict occurs when Git cannot automatically combine changes from two branches. This usually happens when the same part of the same file has been modified differently in multiple branches.

Although merge conflicts may seem intimidating at first, they are a normal part of collaborative software development. Understanding how to identify and resolve conflicts is an essential Git skill.

---

# What is a Merge Conflict?

A merge conflict is a situation where Git cannot decide which version of a file should be kept during a merge.

Instead of making the decision automatically, Git pauses the merge process and asks the developer to resolve the conflict manually.

---

# Why Do Merge Conflicts Occur?

Merge conflicts commonly occur when:

- Two branches modify the same file.
- The same lines are edited differently.
- One branch deletes content while another modifies it.
- Git cannot safely combine the changes.

In these situations, human intervention is required.

---

# Example Scenario

Suppose you have two branches.

```text
main

documentation
```

Both branches modify the same file.

```text
README.md
```

### main branch

```text
Project for Data Analysis
```

### documentation branch

```text
Project for Data Analysis

Created by AI Team
```

Now run:

```bash
git merge documentation
```

Git cannot determine which version should become the final version.

As a result, the merge stops with a conflict.

---

# Understanding the Conflict

Suppose another developer edits the same file while you are working on your own branch.

```text
main
│
├── README.md (edited by Developer A)
│
documentation
│
└── README.md (edited by Developer B)
```

When these branches are merged, Git detects conflicting changes.

---

# Merge Failure

Running:

```bash
git merge documentation
```

may produce output similar to:

```text
CONFLICT (content): Merge conflict in README.md

Automatic merge failed.

Fix conflicts and then commit the result.
```

Git does not complete the merge until the conflict has been resolved.

---

# Opening the Conflicted File

Open the file using:

```bash
nano README.md
```

or use any code editor such as Visual Studio Code.

After opening the file, Git inserts special conflict markers.

---

# Understanding Conflict Markers

Example:

```text
<<<<<<< documentation

Project

Usage Guide

=======

Project

>>>>>>> HEAD
```

These markers help identify the conflicting sections.

---

## <<<<<<< documentation

Everything below this marker belongs to the **documentation** branch.

---

## =======

This line separates the two different versions.

---

## >>>>>>> HEAD

Everything above this marker (after the separator) belongs to the current branch (**HEAD**).

---

# Conflict Visualization

```text
<<<<<<< documentation

Documentation Branch

=======

Current Branch (HEAD)

>>>>>>> HEAD
```

Git adds these markers only to help you decide which content should remain.

---

# Resolving the Conflict

Read both versions carefully.

Choose one of the following:

- Keep the first version.
- Keep the second version.
- Combine both versions.

After deciding, remove all conflict markers.

Example:

Before:

```text
<<<<<<< documentation

Project

Usage Guide

=======

Project

>>>>>>> HEAD
```

After resolving:

```text
Project

Usage Guide
```

The file should contain only the final content.

---

# Saving the File

If you are using Nano:

Save:

```text
Ctrl + O
```

Press:

```text
Enter
```

Exit:

```text
Ctrl + X
```

---

# Completing the Merge

After resolving the conflict:

Stage the updated file.

```bash
git add README.md
```

Create the merge commit.

```bash
git commit
```

Git records the resolved version as the official result of the merge.

---

# Repository Workflow

```text
main
│
│  README.md modified
│
├──────────────┐
│              │
│      documentation
│
│      README.md modified
│
└────── git merge ──────► Merge Conflict
                           │
                           ▼
                  Resolve Conflict
                           │
                           ▼
                     git add README.md
                           │
                           ▼
                       git commit
                           │
                           ▼
                  Merge Completed
```

---

# Preventing Merge Conflicts

Although conflicts are common, they can often be avoided.

Good practices include:

- Pull the latest changes regularly.
- Communicate with teammates.
- Avoid editing the same section of a file simultaneously.
- Create small, focused feature branches.
- Merge branches frequently instead of waiting too long.

As the saying goes:

> Prevention is better than cure.

---

# Real-World Example

Imagine two developers are working on the same portfolio website.

Developer A updates the homepage.

Developer B updates the homepage in another branch.

Both modify the same paragraph.

When the branches are merged, Git cannot determine which version is correct.

The developers manually review both versions, keep the desired content, and complete the merge.

---

# Common Beginner Mistakes

- Deleting the wrong version of the content.
- Forgetting to remove Git conflict markers.
- Forgetting to run `git add` after resolving the conflict.
- Assuming Git resolves every conflict automatically.

---

# Best Practices

- Read both versions before making a decision.
- Remove all conflict markers.
- Test the project after resolving conflicts.
- Commit immediately after resolving a conflict.
- Keep branches small and merge them regularly.

---

# Summary

Merge conflicts occur when Git cannot automatically combine changes from different branches.

Git highlights the conflicting sections using special markers. Developers must manually resolve the differences, remove the markers, stage the updated file, and create a new commit.

Learning to resolve merge conflicts is an essential skill for collaborative software development.

---

# Key Takeaways

- Merge conflicts occur when Git cannot choose between conflicting changes.
- Conflicts usually happen when the same part of the same file is modified.
- Git inserts conflict markers to highlight differences.
- Developers must manually resolve conflicts.
- After resolving, run `git add` followed by `git commit`.

---

# Practice Commands

```bash
git merge documentation

nano README.md

git add README.md

git commit
```

---

# Exercises

- [ ] Create two branches.
- [ ] Modify the same file in both branches.
- [ ] Merge the branches.
- [ ] Identify the conflict markers.
- [ ] Resolve the conflict and complete the merge.

---

# What I Learned

After studying this chapter, I learned:

- What a merge conflict is.
- Why merge conflicts occur.
- How to read Git conflict markers.
- How to resolve conflicts manually.
- The correct workflow for completing a merge after resolving conflicts.