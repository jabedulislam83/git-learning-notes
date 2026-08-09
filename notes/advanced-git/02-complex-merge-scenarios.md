# Complex Merge Scenarios

## Overview

Git provides multiple ways to combine changes from different branches. In the previous chapter, we explored **Fast-forward** and **Recursive** merging.

In this chapter, we explore two additional merge strategies:

- Squash Merge
- Octopus Merge

These strategies are useful in different collaboration and project-history scenarios.

---

# Merge Strategies Covered

So far, we have learned four important merge approaches:

| Merge Type | Main Purpose |
|------------|--------------|
| Fast-forward | Maintain a simple linear history |
| Recursive | Preserve branch history with a merge commit |
| Squash | Combine multiple feature commits into one commit |
| Octopus | Merge three or more independent branches |

---

# Example Project: Flight Travel Data Pipeline

To understand these strategies, imagine that a team is developing a **Flight Travel Data Pipeline**.

Example structure:

```text
flight-data-pipeline/
│
├── etl.py
├── config.py
├── utils.py
│
└── data/
    ├── flights.csv
    └── airports.csv
```

Possible responsibilities:

- `etl.py` → Main ETL pipeline
- `config.py` → Configuration
- `utils.py` → Utility functions
- `data/` → Flight-related datasets

Different developers can work on separate feature branches.

---

# Squash Merge

## What is Squash Merge?

A squash merge combines all commits from a feature branch into a **single new commit** on the target branch.

Suppose a feature branch contains:

```text
A ── B ── C ── D
```

After squashing it into `main`:

```text
A ── B ── S
          ↑
     Squash Commit
```

The new commit `S` contains the changes introduced by the feature branch.

---

# Squash vs Recursive Merge

A normal merge can preserve the individual commits and create a merge commit.

```text
        C ── D
       /     \
A ── B       M
       \     /
        E ──
```

A squash merge instead combines the feature changes into one new commit:

```text
A ── B ── S
          ↑
      One Commit
```

---

# Squash Commit Has One Parent

A squash commit is a normal commit rather than a merge commit.

Therefore, it has one parent.

```text
Parent
   │
   ▼
Squash Commit
```

This differs from a merge commit, which can have multiple parents.

---

# What Happens to the Feature Branch?

Consider:

```text
main
A ── B

feature
     \
      C ── D ── E
```

After squash merging:

```text
main
A ── B ── S
          ↑
      C+D+E changes
```

The original feature branch commits remain on the feature branch.

```text
feature

C ── D ── E
```

The squash operation does not rewrite or remove those original commits from the source branch.

---

# Why Use Squash Merge?

The main advantage of squash merging is a cleaner target-branch history.

Imagine a feature branch contains commits such as:

```text
fix typo
fix typo again
update csv
fix bug
fix bug again
change function
test
test again
cleanup
final fix
```

Keeping every small development commit in `main` may create unnecessary history noise.

Instead, they can become:

```text
Add flight data cleanup process
```

This makes the target branch easier to read.

---

# Squash Merge Example

Suppose we are adding a data-cleaning feature.

The branch is:

```text
data-cleanup
```

It contains several commits:

```text
main

A ── B
      \
       C ── D ── E ── F
```

The feature is now complete.

We want the `main` branch to contain only one clean commit representing the entire feature.

This is a good situation for a squash merge.

---

# Performing a Squash Merge

First, switch to the target branch:

```bash
git switch main
```

Then run:

```bash
git merge --squash data-cleanup
```

Git combines the changes from the feature branch and places them into the staging area.

However, the changes are **not committed automatically**.

Create the commit manually:

```bash
git commit -m "Add flight data cleanup process"
```

---

# Complete Squash Workflow

```bash
git switch main

git merge --squash data-cleanup

git commit -m "Add flight data cleanup process"
```

---

# Important: `git merge --squash` Does Not Commit

This is one of the most important things to remember.

Running:

```bash
git merge --squash data-cleanup
```

does not create a commit.

Instead:

```text
Feature Changes
      │
      ▼
Staging Area
```

You must then run:

```bash
git commit
```

to create the final squash commit.

---

# Squash Merge Result

Before:

```text
main

A ── B
      \
       C ── D ── E
```

After:

```text
main

A ── B ── S
          ↑
      Squash Commit
```

The changes from `C`, `D`, and `E` are represented by `S`.

---

# Squash Merge and History

The target branch becomes simpler:

```text
A ── B ── S
```

However, the feature branch still contains its original development history:

```text
C ── D ── E
```

Therefore:

> Squash merging simplifies the history of the target branch without deleting the original commits from the source branch.

---

# Octopus Merge

## What is an Octopus Merge?

An Octopus merge allows Git to merge **three or more branches at the same time**.

For example:

```text
main
 │
 ├── ingest
 ├── transform
 └── load
```

Instead of merging each branch separately, they can be merged together.

---

# Why is it Called "Octopus"?

The resulting commit graph can have multiple branches coming together at one merge commit.

```text
       ingest ───────\
       transform ─────\
main ───────────────── M
       load ──────────/
```

`M` represents the merge commit.

The graph visually resembles multiple arms coming together, which is why the strategy is called **Octopus**.

---

# Multiple Parents

An Octopus merge creates a single merge commit with multiple parents.

For example:

```text
       ingest ───────\
       transform ────\
main ───────────────── M
       load ──────────/
```

The merge commit `M` has parents corresponding to:

- `main`
- `ingest`
- `transform`
- `load`

This allows several independent branches to be combined in one operation.

---

# Important Condition

Octopus merging is intended for branches that can be merged without conflicts.

If the branches contain conflicting changes, the Octopus merge strategy fails rather than asking you to resolve the conflicts interactively.

Therefore, it is best suited to **independent branches**.

---

# Octopus Merge Example

Consider our Flight Data Pipeline.

The team divides the work into three independent components:

```text
ingest
transform
load
```

The project looks like:

```text
main
 │
 ├── ingest
 ├── transform
 └── load
```

Each branch implements a different part of the pipeline.

If their changes do not conflict, they can be merged together.

---

# Performing an Octopus Merge

First, switch to `main`:

```bash
git switch main
```

Then use:

```bash
git merge -s octopus ingest transform load
```

Here:

| Part | Meaning |
|------|---------|
| `git merge` | Start a merge |
| `-s` | Specify a merge strategy |
| `octopus` | Use the Octopus strategy |
| `ingest` | First branch |
| `transform` | Second branch |
| `load` | Third branch |

---

# Octopus Merge Result

After the merge:

```text
       ingest ───────\
       transform ─────\
main ───────────────── M
       load ──────────/
```

All three feature branches are combined into a single merge commit.

---

# When to Use Each Strategy?

## Squash Merge

Use squash merging when:

- A feature branch contains many small commits.
- You want a clean target-branch history.
- You want to represent an entire feature as one commit.
- Individual development commits are not important on the target branch.

Example:

```text
10 development commits
          ↓
     Squash Merge
          ↓
   1 clean commit
```

---

## Octopus Merge

Use Octopus merging when:

- You need to merge three or more branches.
- The branches contain independent changes.
- The branches can be merged without conflicts.

Example:

```text
Branch 1 ──\
Branch 2 ────> Merge Commit
Branch 3 ──/
```

---

# Four Merge Approaches

We can now compare the four approaches learned so far.

## 1. Fast-forward

```text
A ── B ── C ── D
```

- No new merge commit.
- Linear history.
- Branch pointer moves forward.

---

## 2. Recursive

```text
      C ── D
     /     \
A ── B ──── M
```

- Creates a merge commit.
- Preserves branch history.
- Combines different development paths.

---

## 3. Squash

```text
Feature:

C ── D ── E

      ↓

main:

A ── B ── S
```

- Multiple feature commits become one new commit.
- Simplifies target-branch history.
- Original feature commits remain on the source branch.

---

## 4. Octopus

```text
       branch1 ──\
       branch2 ──── M
       branch3 ──/
```

- Merges three or more branches.
- Creates one merge commit.
- Best for independent, non-conflicting branches.

---

# Squash vs Recursive

This distinction is especially important.

## Recursive

```text
Feature Commits
      │
      ▼
History Preserved
      │
      ▼
Merge Commit
```

The individual feature commits remain visible in the target branch's history.

---

## Squash

```text
Feature Commits
      │
      ▼
Changes Combined
      │
      ▼
One New Commit
```

The target branch receives one new commit containing the combined changes.

---

# Comparison Table

| Feature | Fast-forward | Recursive | Squash | Octopus |
|---------|---------------|-----------|--------|---------|
| New merge commit | ❌ | ✅ | ❌* | ✅ |
| Preserves feature commits on target | ✅ | ✅ | ❌ | ✅ |
| Linear target history | ✅ | ❌ | ✅ | ❌ |
| Multiple branches at once | ❌ | Usually 2 | ❌ | ✅ |
| Best for | Simple branches | Branch integration | Clean history | Independent branches |
| Example | `git merge feature` | `git merge feature` | `git merge --squash feature` | `git merge -s octopus a b c` |

> *`git merge --squash` prepares the changes but requires a separate `git commit`, which creates a normal commit rather than a merge commit.

---

# Common Beginner Mistakes

- Assuming squash merge automatically creates a commit.
- Forgetting to run `git commit` after `git merge --squash`.
- Thinking squash deletes the source branch's original commits.
- Using Octopus merge when branches contain conflicting changes.
- Confusing a squash commit with a merge commit.
- Using complex merge strategies without understanding the resulting history.

---

# Best Practices

- Use squash merging when many development commits create unnecessary noise.
- Use meaningful commit messages for squash commits.
- Use Octopus merging only for independent, non-conflicting branches.
- Understand how each strategy affects project history before choosing it.
- Review the commit graph after complex merges.

---

# Summary

Squash and Octopus are two advanced approaches for combining changes in Git.

**Squash Merge** combines multiple commits from a feature branch into one new commit on the target branch. It is useful for maintaining a clean and readable project history.

**Octopus Merge** combines three or more branches into a single merge commit and is most suitable when the branches contain independent, non-conflicting changes.

---

# Key Takeaways

- Squash Merge converts multiple feature commits into one new commit.
- `git merge --squash` does not automatically create the final commit.
- A squash commit is a normal commit with one parent.
- Squashing simplifies the target branch's history.
- Octopus Merge combines three or more branches at once.
- Octopus merging is best suited to independent branches without conflicts.
- Different merge strategies serve different project-history goals.

---

# Practice Commands

## Squash Merge

```bash
git switch main

git merge --squash data-cleanup

git commit -m "Add flight data cleanup process"
```

## Octopus Merge

```bash
git switch main

git merge -s octopus ingest transform load
```

---

# Exercises

- [ ] Create a feature branch with several commits.
- [ ] Perform a squash merge into `main`.
- [ ] Inspect the resulting commit history.
- [ ] Verify that the original feature commits remain on the feature branch.
- [ ] Create three independent branches.
- [ ] Merge them using the Octopus strategy.
- [ ] Observe the resulting commit graph.
- [ ] Explain when Octopus merging should and should not be used.

---

# What I Learned

After studying this chapter, I learned:

- How Squash Merge works.
- How to combine multiple feature commits into one commit.
- Why squash merging can reduce history noise.
- The difference between a squash commit and a merge commit.
- How Octopus Merge combines multiple branches.
- Why Octopus Merge is best suited for independent, non-conflicting branches.
- How different merge strategies affect Git history.