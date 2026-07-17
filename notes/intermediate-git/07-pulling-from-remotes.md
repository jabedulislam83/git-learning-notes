# Pulling from Remotes

## Overview

In collaborative Git workflows, developers continuously exchange changes between their local repositories and a shared remote repository. While cloning downloads a repository for the first time, keeping that local copy up to date requires additional Git commands.

Git provides `git fetch`, `git merge`, and `git pull` to synchronize changes between remote and local repositories.

Understanding the difference between these commands is essential for effective collaboration.

---

# Why Synchronization is Important

In a team project, developers work independently on their own computers.

Meanwhile, teammates continue pushing new commits to the remote repository.

As a result, your local repository can become outdated.

```text
Developer A
        │
        ▼
      GitHub
        ▲
        │
Developer B
```

To continue working with the latest version of the project, you must synchronize your local repository with the remote repository.

---

# Local vs Remote Changes

Suppose your local repository contains:

```text
Retail Project

├── sales.csv
└── app.py
```

Meanwhile, another developer pushes new files.

The remote repository now contains:

```text
Retail Project

├── sales.csv
├── app.py
├── inventory.csv
└── dashboard/
```

Your local repository does not automatically receive these changes.

You must retrieve them manually.

---

# Remote Repository as the Source of Truth

In collaborative development, the remote repository is considered the **source of truth**.

It stores:

- The latest approved code
- Shared project history
- Contributions from all developers

Every team member synchronizes their local repository with the remote repository to stay up to date.

---

# Fetching Changes

To download updates from a remote repository without modifying your current branch, use:

```bash
git fetch origin
```

Git downloads:

- New commits
- Updated branches
- Repository metadata

However, your working files remain unchanged.

---

# What Does `git fetch` Do?

After running:

```bash
git fetch origin
```

Git updates its local copy of the remote repository.

It **does not** merge those changes into your current branch.

Workflow:

```text
Remote Repository
        │
        ▼
   git fetch
        │
        ▼
Local Git Database Updated

Current Branch
(No Changes Yet)
```

This makes `git fetch` a safe way to inspect incoming changes before applying them.

---

# Fetching a Specific Branch

Instead of downloading every branch, you can fetch only one.

```bash
git fetch origin main
```

Git downloads updates only for the **main** branch.

---

# Synchronizing with Merge

After fetching, the downloaded commits exist only inside Git's local database.

To update your current branch, perform a merge.

Example:

```bash
git merge origin/main
```

Git combines the fetched changes with your current branch.

Workflow:

```text
Remote
   │
git fetch
   │
   ▼
Local Git Database
   │
git merge
   ▼
Current Branch Updated
```

---

# Pulling Changes

Because developers frequently perform **fetch** followed by **merge**, Git provides a shortcut.

```bash
git pull origin
```

This command automatically performs:

```bash
git fetch origin

git merge
```

Both operations are executed in a single command.

---

# How `git pull` Works

Workflow:

```text
Remote Repository
        │
        ▼
     git pull
        │
        ▼
Download Updates

        │
        ▼
Merge Updates

        │
        ▼
Current Branch Updated
```

This is the most common synchronization command used in everyday Git development.

---

# Pulling a Specific Branch

You can pull updates from a specific remote branch.

Example:

```bash
git pull origin dev
```

Before running this command, make sure you are on the corresponding local branch.

```bash
git switch dev

git pull origin dev
```

Otherwise, Git attempts to merge the remote branch into whichever branch you are currently using.

---

# Understanding Pull Output

The output of `git pull` usually contains information from both:

- Fetch
- Merge

You may see:

- Updated commit IDs
- Changed files
- Fast-forward merges
- Merge summaries

Because `git pull` combines two Git operations into one.

---

# Fetch vs Pull

| Feature | `git fetch` | `git pull` |
|---------|-------------|------------|
| Downloads updates | ✅ | ✅ |
| Merges automatically | ❌ | ✅ |
| Updates current branch | ❌ | ✅ |
| Safe for inspection | ✅ | Less safe |
| Most commonly used before reviewing changes | ✅ | ❌ |

---

# When Should You Use Fetch?

Use **fetch** when you want to:

- Review incoming changes first.
- Compare branches.
- Avoid automatic merges.
- Inspect updates before modifying your working branch.

---

# When Should You Use Pull?

Use **pull** when:

- You trust the incoming changes.
- You want your branch updated immediately.
- You are regularly synchronizing with teammates.

---

# A Word of Caution

Suppose you have modified several files locally but have not committed them.

If you run:

```bash
git pull
```

Git may stop the operation because your uncommitted changes could conflict with incoming updates.

You may receive a message indicating that the pull has been aborted.

Always save your work before pulling.

```bash
git add .

git commit -m "Save local changes"
```

Then run:

```bash
git pull origin
```

---

# Real-World Example

Imagine you are working on your **Retail Intelligence Platform** project.

Your teammate adds a new dashboard feature and pushes it to GitHub.

Before continuing your own work, update your local repository.

```bash
git pull origin main
```

Now your project includes the latest dashboard implementation, allowing everyone to work on the same codebase.

---

# Common Beginner Mistakes

- Assuming `git fetch` updates working files.
- Forgetting to pull before starting new work.
- Pulling while having uncommitted changes.
- Pulling into the wrong branch.
- Confusing `fetch` with `pull`.

---

# Best Practices

- Pull the latest changes before starting work each day.
- Commit your local changes before pulling.
- Use `git fetch` when you want to inspect updates first.
- Use `git pull` for quick synchronization.
- Always verify which branch you are currently using before pulling.

---

# Summary

Keeping your local repository synchronized with the remote repository is essential for collaborative development.

`git fetch` downloads updates without modifying your current branch, while `git pull` combines fetching and merging into a single command.

Understanding when to use each command helps prevent conflicts and keeps your project synchronized.

---

# Key Takeaways

- Remote repositories continuously receive new updates.
- `git fetch` downloads changes without merging them.
- `git merge` integrates fetched changes into your branch.
- `git pull` performs both operations automatically.
- Commit local work before pulling to avoid conflicts.

---

# Practice Commands

```bash
git fetch origin

git fetch origin main

git merge origin/main

git pull origin

git switch dev

git pull origin dev
```

---

# Exercises

- [ ] Explain the difference between `git fetch` and `git pull`.
- [ ] Fetch updates from the `main` branch.
- [ ] Merge fetched changes into your current branch.
- [ ] Pull updates from a remote repository.
- [ ] Describe why the remote repository is called the "source of truth."

---

# What I Learned

After studying this chapter, I learned:

- How remote and local repositories stay synchronized.
- The purpose of `git fetch`.
- How `git merge` applies fetched changes.
- Why `git pull` is a shortcut for fetching and merging.
- Best practices for safely updating a local repository.