# Pushing to Remotes

## Overview

After learning how to download changes from a remote repository using `git pull`, the next step is to upload your own work. Git uses the `git push` command to send local commits to a remote repository.

Pushing allows your teammates to access your latest changes and keeps the shared project up to date. It is one of the most frequently used commands in collaborative Git workflows.

---

# What is `git push`?

The `git push` command uploads commits from your local repository to a remote repository.

Unlike `git pull`, which brings changes from the remote to your computer, `git push` sends your local work to the shared repository.

Workflow:

```text
Local Repository
        │
   git push
        ▼
Remote Repository
```

Only committed changes are pushed to the remote repository.

---

# Before You Push

Before running `git push`, your changes must already exist in your Git history.

Typical workflow:

```text
Edit Files
     │
git add
     │
git commit
     │
git push
```

If you have uncommitted changes, Git has nothing to upload.

---

# Basic Push Command

The general syntax is:

```bash
git push <remote> <branch>
```

Example:

```bash
git push origin main
```

Here:

- `origin` is the remote repository.
- `main` is the local branch being pushed.

Git uploads the commits from your local **main** branch to the remote repository.

---

# Understanding the Push Workflow

In collaborative projects, developers usually follow the same workflow repeatedly.

```text
Remote Repository
        │
   git pull
        ▼
Local Repository
        │
   Make Changes
        │
   git add
        │
   git commit
        │
   git push
        ▼
Remote Repository
```

This cycle keeps every developer synchronized with the latest version of the project.

---

# Why Pull Before Pushing?

Suppose another developer has already pushed new commits.

Your repository now looks like this.

Remote:

```text
A ─── B ─── C
```

Local:

```text
A ─── B
        \
         D
```

Your local repository does not contain commit **C**.

If you immediately run:

```bash
git push origin main
```

Git rejects the push.

---

# Understanding Push Rejection

A rejected push usually means your local branch is behind the remote branch.

Git may display a message similar to:

```text
Rejected

Failed to push

Updates were rejected because the remote contains work that you do not have locally.
```

Git prevents the push because uploading your branch could overwrite another developer's work.

---

# Resolving a Push Rejection

The recommended solution is:

```bash
git pull origin main
```

Git downloads the missing commits and merges them into your local branch.

After the merge is complete, push again.

```bash
git push origin main
```

This keeps both repositories synchronized.

---

# Fast-Forward vs Recursive Merge

Sometimes pulling performs a **Fast-forward Merge**.

```text
Remote

A ─── B ─── C

Local

A ─── B
```

Git simply moves your branch forward.

---

However, if both repositories contain different commits:

```text
Remote

A ─── B ─── C

Local

A ─── B ─── D
```

Git cannot fast-forward.

Instead, Git performs a merge that combines both histories.

A merge commit is created to preserve both sets of changes.

---

# Merge Commit Message

During some merges, Git automatically opens your default text editor.

You will be asked to confirm or edit the merge commit message.

Save the file and exit the editor to complete the merge.

---

# Using `--no-edit`

Git also allows you to accept the default merge message automatically.

Example:

```bash
git pull --no-edit origin main
```

This skips editing the merge commit message.

Although convenient, writing meaningful commit messages is generally considered better practice.

---

# Pushing a New Branch

Suppose you create a new local branch.

```text
feature-login
```

This branch does not yet exist on GitHub.

Simply run:

```bash
git push origin feature-login
```

Git automatically creates a new branch with the same name on the remote repository.

Result:

```text
Remote Repository

main

feature-login
```

Your teammates can now access this branch.

---

# Push vs Pull

| Feature | `git pull` | `git push` |
|---------|------------|------------|
| Direction | Remote → Local | Local → Remote |
| Purpose | Download updates | Upload commits |
| Synchronizes repository | Yes | Yes |
| Requires committed changes | No | Yes |

---

# Real-World Example

Suppose you are developing a **Retail Intelligence Platform** with your team.

At the beginning of the day, you download the latest updates.

```bash
git pull origin main
```

You implement a new inventory feature.

```bash
git add .

git commit -m "Add inventory dashboard"
```

Finally, share your work with the team.

```bash
git push origin main
```

Now every teammate can access your new feature after pulling from the remote repository.

---

# Common Beginner Mistakes

- Forgetting to commit before pushing.
- Pushing without pulling recent changes.
- Ignoring push rejection messages.
- Pushing to the wrong branch.
- Assuming uncommitted files will be uploaded.

---

# Best Practices

- Pull the latest changes before starting work.
- Commit frequently with meaningful messages.
- Push only tested and stable code.
- Read Git's error messages carefully.
- Keep your local repository synchronized with the remote repository.

---

# Summary

The `git push` command uploads committed changes from your local repository to a remote repository.

In collaborative development, developers typically pull the latest changes, complete their work locally, commit those changes, and finally push them to the shared repository.

Understanding the push workflow helps prevent synchronization issues and ensures smooth collaboration.

---

# Key Takeaways

- `git push` uploads committed changes to a remote repository.
- Push requires committed changes.
- Always pull recent updates before pushing.
- Pushes may be rejected if your branch is behind the remote.
- New local branches can be published using `git push origin <branch>`.

---

# Practice Commands

```bash
git push origin main

git pull origin main

git pull --no-edit origin main

git push origin feature-login
```

---

# Exercises

- [ ] Push a local commit to the `main` branch.
- [ ] Create a new local branch.
- [ ] Publish the new branch to GitHub.
- [ ] Explain why a push may be rejected.
- [ ] Describe the recommended pull → commit → push workflow.

---

# What I Learned

After studying this chapter, I learned:

- How `git push` uploads local commits to a remote repository.
- Why commits must exist before pushing.
- How to resolve push rejections.
- The difference between fast-forward and merge-based synchronization.
- How to publish a new branch to a remote repository.