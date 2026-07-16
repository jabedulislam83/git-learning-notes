# Working with Branches

## Overview

After understanding what branches are and why they are important, the next step is learning how to work with them.

Git provides simple commands to create, switch, and manage branches. These commands allow developers to work on different features independently while keeping the main branch stable.

---

# Visualizing Branches

A Git repository starts with a single branch.

```text
main

A ─── B ─── C
```

Each letter represents a commit.

- A → First commit
- B → Second commit
- C → Third commit

At this point, all development happens on the **main** branch.

---

# Branching Off

Suppose you want to build a new feature called **AI Assistant**.

Instead of modifying the main branch directly, create a new branch.

```text
main

A ─── B ─── C
             \
              \
               D
```

The new branch starts from the latest commit of the main branch.

This process is called **Branching Off**.

---

# Continuing Development

Once the branch is created, development continues independently.

```text
main

A ─── B ─── C

             \
              \
               D ─── E ─── F

      ai-assistant
```

Notice that:

- The **main** branch remains unchanged.
- All new commits are added only to the feature branch.

---

# Viewing Existing Branches

Display all local branches.

```bash
git branch
```

Example:

```text
* main
  ai-assistant
```

The `*` symbol indicates the current branch.

In this example, you are currently working on **main**.

---

# Switching Between Branches

Move from one branch to another.

```bash
git switch ai-assistant
```

Output:

```text
Switched to branch 'ai-assistant'
```

Verify:

```bash
git branch
```

Output:

```text
* ai-assistant
  main
```

Now every new commit will be added to the **ai-assistant** branch.

---

# Returning to the Main Branch

Switch back to the main branch.

```bash
git switch main
```

Output:

```text
Switched to branch 'main'
```

---

# Creating a New Branch

Create a branch.

```bash
git branch contact-page
```

Check:

```bash
git branch
```

Output:

```text
* main
  contact-page
```

Notice that Git creates the branch but keeps you on the current branch.

---

# Creating and Switching at the Same Time

Instead of running two commands,

```bash
git branch chatbot

git switch chatbot
```

Git provides a shortcut.

```bash
git switch -c chatbot
```

This command:

1. Creates the branch.
2. Switches to it immediately.

---

# Older Command

Older Git tutorials often use:

```bash
git checkout -b chatbot
```

Modern Git recommends:

```bash
git switch -c chatbot
```

because it is easier to understand and clearly describes its purpose.

---

# Understanding Branching Off

Suppose your repository contains:

```text
main

A ─── B ─── C
```

Create a new branch.

```bash
git switch -c feature-login
```

Now the repository becomes:

```text
main

A ─── B ─── C
             \
              \
               D

feature-login
```

We say:

> **The feature-login branch was branched off from the main branch.**

---

# Real-World Example

Imagine you are building your portfolio website.

Current pages:

```text
Home

About

Projects
```

You want to develop a Contact page.

Instead of editing the live website, create a new branch.

```bash
git switch -c contact-page
```

Develop the Contact page independently.

```text
main

Home
About
Projects
```

```text
contact-page

Home
About
Projects
Contact
```

After testing, the branch will later be merged into the main branch.

---

# Common Beginner Mistakes

- Forgetting to switch to the correct branch.
- Thinking `git branch` automatically switches branches.
- Creating multiple features in the same branch.
- Working directly on the main branch.

---

# Best Practices

- Create one branch for one feature.
- Use meaningful branch names.
- Verify your current branch before committing.
- Prefer `git switch` over the older `git checkout`.
- Delete feature branches after merging.

---

# Summary

Git branches allow developers to work safely and independently.

Commands like `git branch`, `git switch`, and `git switch -c` simplify branch management and make collaboration easier.

Understanding how to create and switch branches is essential before learning how to compare and merge them.

---

# Key Takeaways

- `git branch` displays existing branches.
- `git branch <name>` creates a new branch.
- `git switch <branch>` changes the current branch.
- `git switch -c <name>` creates and switches to a branch.
- One branch should focus on one feature.

---

# Practice Commands

```bash
git branch

git branch feature-login

git switch feature-login

git switch main

git switch -c contact-page
```

---

# Exercises

- [ ] Create a new branch named `feature-login`.
- [ ] Switch to the new branch.
- [ ] Return to the main branch.
- [ ] Create another branch using `git switch -c`.
- [ ] Verify the current branch using `git branch`.

---

# What I Learned

After studying this chapter, I learned:

- How to view existing branches.
- How to create new branches.
- How to switch between branches.
- The difference between `git branch` and `git switch`.
- Why `git switch -c` is the preferred modern approach.