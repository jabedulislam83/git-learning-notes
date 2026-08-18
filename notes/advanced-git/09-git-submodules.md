# Git Submodules

## Overview

Modern software projects often depend on external libraries, shared components, or separate repositories.

Sometimes we want to include another Git repository inside our main project while still keeping its Git history and versioning separate.

Git provides a feature called **`git submodule`** for this purpose.

A Git submodule allows one Git repository to be included inside another Git repository.

---

# What is Git Submodule?

A **Git submodule** is a Git repository nested inside another Git repository.

For example, suppose we have a main project:

```text
data-pipeline/
├── src/
├── data/
├── notebooks/
└── libs/
```

Now suppose we have a separate repository containing a data validation library:

```text
validator/
```

We can include it inside our main project:

```text
data-pipeline/
├── src/
├── data/
├── notebooks/
└── libs/
    └── validator/
```

Here:

```text
data-pipeline → Main Repository
validator     → Submodule
```

The submodule remains an **independent Git repository**.

It has:

* Its own Git history
* Its own commits
* Its own branches
* Its own remote repository

---

# Main Repository vs Submodule

The main repository does not copy the entire history of the submodule.

Instead, it records a **specific commit** of the submodule.

For example:

```text
Main Repository
      ↓
validator
      ↓
Commit: a8f42c1
```

This means:

> The main project is using the version of the submodule represented by commit `a8f42c1`.

Conceptually:

```text
Main Repository
       │
       │ records a specific commit
       ↓
Submodule Repository
       │
       ├── Commit A
       ├── Commit B
       ├── Commit C
       └── Commit D
```

This is one of the most important concepts of Git Submodules.

---

# Why Use Git Submodules?

Git Submodules are useful when a project needs external or shared code that is maintained separately.

For example:

```text
Main Data Project
       ↓
Data Validation Library
       ↓
Separate Git Repository
```

Instead of copying the validation library into the main project, we can add it as a submodule.

### Advantages

* Keep external code separate
* Maintain separate Git history
* Use a specific version of a dependency
* Share the same repository across multiple projects
* Keep the main repository organized
* Allow different teams to maintain different components

---

# Adding a Submodule

To add a submodule:

```bash
git submodule add <repository-url> <path>
```

Example:

```bash
git submodule add https://github.com/example/validator.git libs/validator
```

After adding it, the project may look like:

```text
data-pipeline/
├── src/
├── data/
├── notebooks/
├── libs/
│   └── validator/
└── .gitmodules
```

Git creates a `.gitmodules` file to store the submodule configuration.

---

# What is `.gitmodules`?

The `.gitmodules` file contains information about the submodules used by the project.

Example:

```ini
[submodule "libs/validator"]
    path = libs/validator
    url = https://github.com/example/validator.git
```

The `path` specifies where the submodule is located:

```text
libs/validator
```

The `url` specifies the submodule repository:

```text
https://github.com/example/validator.git
```

So:

```text
.gitmodules
     ↓
Submodule Configuration
     ↓
Path + Repository URL
```

---

# Checking Submodule Status

To see the current submodule information:

```bash
git submodule status
```

Example:

```text
a8f42c1 libs/validator
```

Here:

```text
a8f42c1
```

is the commit currently referenced by the submodule.

And:

```text
libs/validator
```

is the submodule path.

---

# Cloning a Repository with Submodules

Suppose someone clones the main repository:

```bash
git clone <main-repository-url>
```

The main repository is cloned, but the submodule may not be initialized automatically.

The easiest way to clone the main repository together with its submodules is:

```bash
git clone --recurse-submodules <main-repository-url>
```

This performs:

```text
Clone Main Repository
        ↓
Initialize Submodules
        ↓
Clone Submodule Repositories
```

---

# Initializing Existing Submodules

If the main repository has already been cloned normally:

```bash
git clone <main-repository-url>
```

initialize the submodules with:

```bash
git submodule update --init
```

For nested submodules:

```bash
git submodule update --init --recursive
```

The `--recursive` option is useful when a submodule itself contains another submodule.

Example:

```text
Main Repository
      ↓
Submodule A
      ↓
Submodule B
```

Using:

```bash
git submodule update --init --recursive
```

initializes all of them.

---

# Updating a Submodule

There are two important ways to update a submodule.

## Updating to the Recorded Commit

Use:

```bash
git submodule update
```

This checks out the commit that the main repository currently records for the submodule.

Conceptually:

```text
Main Repository
      ↓
Recorded Commit
      ↓
Submodule
```

---

## Updating from the Remote Repository

Use:

```bash
git submodule update --remote
```

This updates the submodule from its configured remote branch.

For a specific submodule:

```bash
git submodule update --remote libs/validator
```

### Important Difference

```text
git submodule update
        ↓
Use commit recorded by main repository
```

while:

```text
git submodule update --remote
        ↓
Get a newer commit from the remote repository
```

---

# Working Inside a Submodule

A submodule is itself a Git repository.

Therefore, we can enter the submodule:

```bash
cd libs/validator
```

Then use normal Git commands:

```bash
git status
git add .
git commit -m "Update validation logic"
git push
```

These commits belong to the **submodule repository**.

They do not directly become commits in the main repository.

Think of them as two separate repositories:

```text
Main Repository
       ≠
Submodule Repository
```

---

# Updating the Main Repository

Suppose the main repository currently points to:

```text
validator → Commit A
```

Now we make a new commit inside the submodule:

```text
Commit A → Commit B
```

The submodule is now at Commit B, but the main repository still points to Commit A.

Run:

```bash
git status
```

The main repository may show:

```text
modified: libs/validator
```

To update the main repository's reference:

```bash
git add libs/validator
git commit -m "Update validator submodule"
git push
```

Now the main repository records:

```text
validator → Commit B
```

The important idea is:

```text
Submodule Repository
        ↓
New Commit
        ↓
Main Repository detects new commit
        ↓
git add
        ↓
git commit
        ↓
git push
```

---

# Manual Submodule Workflow

A typical workflow looks like:

```text
Add Submodule
      ↓
git submodule add
      ↓
Commit Main Repository
      ↓
Clone Project Later
      ↓
Initialize Submodule
      ↓
Work Inside Submodule
      ↓
Commit + Push Submodule
      ↓
Update Main Repository
      ↓
Commit + Push Main Repository
```

The important point is that **both repositories have their own commits**.

---

# Removing a Submodule

Removing a submodule requires a few steps.

First, deinitialize it:

```bash
git submodule deinit -f libs/validator
```

Then remove it from the repository:

```bash
git rm -f libs/validator
```

Check the changes:

```bash
git status
```

Then commit:

```bash
git commit -m "Remove validator submodule"
```

Finally:

```bash
git push
```

---

# When to Use Git Submodules

Git Submodules are useful when:

* You need an external Git repository inside your project.
* You need to lock a dependency to a specific commit.
* The same code is shared across multiple projects.
* Different teams maintain different components.
* The dependency has its own Git history.
* The external project does not need extremely frequent updates.

Example:

```text
Main Project
     ↓
ML Utilities
     ↓
Data Validation Library
```

The validation library can be maintained independently as a submodule.

---

# When to Avoid Git Submodules

Submodules can become inconvenient when a dependency changes very frequently.

For example:

```text
Daily Updates
      +
Frequent Commits
      +
Rapid Development
```

In such situations, a package manager may be simpler.

Examples:

```text
Python → pip
Node.js → npm
```

Therefore, choose the tool based on the project's requirements.

---

# Important Commands

| Command                                   | Purpose                                |
| ----------------------------------------- | -------------------------------------- |
| `git submodule add <url> <path>`          | Add a submodule                        |
| `git submodule status`                    | Check submodule status                 |
| `git submodule update --init`             | Initialize submodules                  |
| `git submodule update --init --recursive` | Initialize nested submodules           |
| `git clone --recurse-submodules <url>`    | Clone repository with submodules       |
| `git submodule update`                    | Checkout the recorded submodule commit |
| `git submodule update --remote`           | Update from the remote repository      |
| `git submodule deinit -f <path>`          | Deinitialize a submodule               |
| `git rm -f <path>`                        | Remove a submodule                     |

---

# Common Beginner Mistakes

* Forgetting to initialize submodules after cloning.
* Using `git submodule update` when you actually need the latest remote version.
* Forgetting that the submodule has its own Git history.
* Making a commit inside the submodule but forgetting to update the main repository.
* Forgetting to push the submodule's commit to its remote repository.
* Forgetting to run `git bisect reset` — wait, this applies to bisect, not submodules.

For submodules, the key thing to remember is:

> **A submodule update and a main repository update are separate Git operations.**

---

# Best Practices

* Use submodules when the dependency genuinely needs to remain a separate Git repository.
* Keep the submodule at a known, stable commit.
* Document important submodules in the project README.
* Initialize submodules after cloning when necessary.
* Push submodule commits before updating the main repository's reference.
* Use `--recursive` when nested submodules are involved.
* Avoid submodules for dependencies that are better managed by package managers.

---

# Complete Example

Suppose we have:

```text
data-pipeline/
```

and want to include:

```text
validator/
```

as a submodule.

## Step 1 — Add the Submodule

```bash
git submodule add https://github.com/example/validator.git libs/validator
```

## Step 2 — Commit the Main Repository

```bash
git add .gitmodules libs/validator
git commit -m "Add validator submodule"
git push
```

## Step 3 — Clone the Project Later

```bash
git clone --recurse-submodules <main-repository-url>
```

## Step 4 — Work Inside the Submodule

```bash
cd libs/validator
```

Make changes and commit them:

```bash
git add .
git commit -m "Update validation logic"
git push
```

## Step 5 — Update the Main Repository

Go back:

```bash
cd ../..
```

Then:

```bash
git add libs/validator
git commit -m "Update validator submodule"
git push
```

Now the main repository points to the new submodule commit.

---

# Summary

Git Submodule allows one Git repository to contain another independent Git repository.

The submodule maintains its own:

* Git history
* Commits
* Branches
* Remote repository

The main repository does not store the complete history of the submodule. Instead, it records a **specific commit** of that submodule.

Submodules are useful for external libraries, shared components, and projects that need independent version control.

For cloning:

```bash
git clone --recurse-submodules <url>
```

For initializing:

```bash
git submodule update --init --recursive
```

For updating from remote:

```bash
git submodule update --remote
```

After changing the submodule, remember to update the main repository's reference as well.

---

# Key Takeaways

* A **Git Submodule** is a Git repository inside another Git repository.
* A submodule has its own Git history and commits.
* The main repository tracks a **specific commit** of the submodule.
* `.gitmodules` stores the submodule's path and repository URL.
* `git clone --recurse-submodules` clones a repository with its submodules.
* `git submodule update --init --recursive` initializes submodules.
* `git submodule update --remote` updates a submodule from its remote repository.
* Changes inside a submodule and changes to the main repository are separate.
* Submodules are useful for external or shared Git repositories.
* Package managers may be better for frequently updated dependencies.

---

# Practice Commands

## Add a Submodule

```bash
git submodule add <url> <path>
```

## Check Submodule Status

```bash
git submodule status
```

## Initialize Submodules

```bash
git submodule update --init --recursive
```

## Clone with Submodules

```bash
git clone --recurse-submodules <url>
```

## Update from Remote

```bash
git submodule update --remote
```

## Deinitialize a Submodule

```bash
git submodule deinit -f <path>
```

## Remove a Submodule

```bash
git rm -f <path>
```

---

# Exercises

* [ ] Create a main Git repository.
* [ ] Add another Git repository as a submodule.
* [ ] Check the submodule using `git submodule status`.
* [ ] Clone the project using `--recurse-submodules`.
* [ ] Practice initializing a submodule with `git submodule update --init --recursive`.
* [ ] Make a change inside the submodule and commit it.
* [ ] Update the main repository to reference the new submodule commit.
* [ ] Practice updating a submodule using `git submodule update --remote`.
* [ ] Remove the submodule using the proper Git commands.

---

# What I Learned

After studying this chapter, I learned:

* What Git Submodules are.
* How a repository can contain another Git repository.
* How the main repository tracks a specific submodule commit.
* What the `.gitmodules` file does.
* How to add a submodule.
* How to clone and initialize submodules.
* How to update submodules.
* How to work inside a submodule.
* How to update the main repository after changing a submodule.
* How to remove a submodule.
* When Git Submodules are useful.
* When a package manager may be a better choice.
