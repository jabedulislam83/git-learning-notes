# Branches

## Overview

Branches are one of Git's most powerful features. They allow developers to work on new features, fix bugs, or experiment with ideas without affecting the main project.

Instead of making changes directly to the production code, developers create separate branches to work independently. Once the work is complete and tested, the changes can be merged back into the main branch.

---

# What is a Branch?

A branch is an independent line of development within a Git repository.

Think of a branch as another version of your project where you can safely make changes without affecting the original version.

Git creates a separate pointer for each branch, allowing multiple versions of the same project to exist simultaneously.

---

# Understanding Branches with an Example

Suppose your project looks like this.

```text
Portfolio Website

├── Home
├── About
├── Projects
└── README.md
```

Everything is working correctly.

Now you want to add a **Contact Page**.

Instead of editing the main project directly, you create a new branch.

```text
main

Home
About
Projects
README
```

↓

```text
contact-page

Home
About
Projects
README
Contact
```

The **main** branch remains unchanged while development continues safely in the new branch.

---

# Branches as Parallel Universes

One of the easiest ways to understand branches is to imagine parallel universes.

```text
Universe A

main

Calculator
```

↓

```text
Universe B

feature-dark-mode

Calculator
+
Dark Mode
```

If something goes wrong in **feature-dark-mode**, the **main** branch remains completely unaffected.

This isolation makes Git branches safe for experimentation.

---

# Why Do We Use Branches?

Branches solve several common software development problems.

They allow developers to:

- Develop new features safely
- Fix bugs without affecting production
- Experiment with new ideas
- Work on multiple tasks simultaneously
- Collaborate with other developers

Without branches, every change would directly affect the production code.

---

# Default Branch

Every Git repository has a default branch.

Today, the standard default branch is:

```text
main
```

The main branch usually contains:

- Stable code
- Production-ready code
- The official version of the project

Developers normally avoid making experimental changes directly on the main branch.

---

# Feature Branches

Each new feature should be developed in its own branch.

Example:

```text
main

├── login-feature
├── payment-feature
├── dark-mode
├── chatbot
└── bug-fix-navbar
```

Keeping one feature per branch makes projects easier to manage.

---

# Working Independently

Suppose three developers are working on the same project.

```text
Developer A

login-feature
```

```text
Developer B

payment-feature
```

```text
Developer C

dashboard
```

Each developer works in a separate branch.

No one overwrites another developer's work.

After completing their tasks, the branches are merged into the main branch.

---

# Real-World Example

Imagine you are developing your portfolio website.

Current website:

```text
Home

About

Projects
```

You decide to add a Contact page.

Instead of changing the main branch, you create:

```text
contact-page
```

After implementing and testing the Contact page, the branch can later be merged into the main branch.

This keeps the live website stable throughout development.

---

# Branch Workflow

A typical Git workflow looks like this.

```text
main
        │
        ▼
Create Branch
        │
        ▼
Develop Feature
        │
        ▼
Test Changes
        │
        ▼
Merge into main
```

This workflow is widely used in professional software development.

---

# Best Practices

- Create one branch for one task.
- Keep the main branch stable.
- Use meaningful branch names.
- Delete feature branches after merging.
- Test changes before merging.

---

# Summary

Branches allow developers to work independently without affecting the main project.

They make software development safer, support collaboration, and simplify feature development and bug fixing.

Learning how to use branches is one of the most important skills in Git.

---

# Key Takeaways

- A branch is an independent line of development.
- The default branch is usually **main**.
- Branches protect the production code.
- One feature should be developed in one branch.
- Branches enable multiple developers to work simultaneously.

---

# Practice Commands

Display all local branches.

```bash
git branch
```

Create a new branch.

```bash
git branch contact-page
```

Display the current branch.

```bash
git branch
```

---

# Exercises

- [ ] Explain what a Git branch is.
- [ ] Describe why developers use branches.
- [ ] List three advantages of using branches.
- [ ] Draw a simple branch diagram.
- [ ] Identify the default branch in a Git repository.

---

# What I Learned

After studying this chapter, I learned:

- What a Git branch is.
- Why branches are important.
- The purpose of the main branch.
- How branches improve collaboration.
- Why each branch should have a single purpose.