# Git Bisect

## Overview

Sometimes a bug is not immediately apparent. A project may work correctly at one point in its history but fail after many later commits.

When a project contains many commits, manually checking every commit to find when the bug was introduced can be time-consuming.

Git provides a debugging tool called **`git bisect`** that uses **binary search** to efficiently identify the first commit that introduced a bug.

---

# What is `git bisect`?

`git bisect` is a Git debugging tool that performs a binary search through a project's commit history.

Its goal is to find the:

> **First Bad Commit**

That is, the first commit where the bug appeared.

---

# Why Binary Search?

Suppose a project has 100 commits:

```text
C1 C2 C3 C4 ... C100
```

Checking every commit one by one could take a long time.

Instead, `git bisect` checks a commit around the middle:

```text
C1 ───────── C50 ───────── C100
             ↑
          First Test
```

If `C50` is bad:

```text
C1 ───── C50 ───── C100
          ↑
         Bad
```

Git knows that the first bad commit must be somewhere before or at `C50`.

It then searches the smaller range.

For example:

```text
100
 ↓
50
 ↓
25
 ↓
12
 ↓
6
 ↓
3
 ↓
1
```

This is the power of binary search.

---

# Why is it Called a "Time Machine"?

Git can move the working tree to older commits during a bisect session.

Conceptually:

```text
Current Version
      ↓
Older Commit
      ↓
Test
      ↓
Another Commit
      ↓
Test
```

This allows us to test different historical states of the project.

---

# Example: Data Pipeline Regression

Imagine a data pipeline:

```text
CSV
 ↓
ETL
 ↓
Database
 ↓
Report
```

The pipeline used to produce correct results.

Now it produces incorrect results:

```text
CSV
 ↓
ETL
 ↓
Database
 ↓
Wrong Results
```

Suppose 200 commits were made between the last known working version and the current version.

Instead of checking all 200 commits manually, `git bisect` can efficiently narrow down the problematic commit.

---

# Starting Bisect

Begin a bisect session with:

```bash
git bisect start
```

Git now enters **Bisect Mode**.

---

# Marking the Current Commit as Bad

Suppose the current commit contains the bug.

Run:

```bash
git bisect bad
```

This tells Git:

> The current commit is broken.

---

# Identifying a Known Good Commit

Now we need an older commit that we know was working correctly.

Suppose its hash is:

```text
abc123
```

Run:

```bash
git bisect good abc123
```

This tells Git:

> This commit does not contain the bug.

---

# Defining the Search Range

We have now given Git two boundaries:

```text
GOOD                         BAD
 ↓                            ↓
C1 ── C2 ── C3 ── C4 ── C5 ── C6 ── C7
```

Git knows:

```text
C1 = Good
C7 = Bad
```

The first bad commit must be somewhere between these two points.

---

# How Bisect Searches

Git selects a commit approximately in the middle of the search range.

For example:

```text
C1 ── C2 ── C3 ── C4 ── C5 ── C6 ── C7
                       ↑
                  Git selects
```

Git checks out that commit.

You then test the project.

---

# If the Commit is Bad

If the bug exists in the selected commit:

```bash
git bisect bad
```

This tells Git:

> The selected commit is bad.

Git then continues searching the earlier half of the history.

---

# If the Commit is Good

If the project works correctly at the selected commit:

```bash
git bisect good
```

This tells Git:

> The selected commit is good.

Git then searches the later half.

---

# Repeating the Process

The process continues:

```text
Select Commit
      ↓
Test
      ↓
Good or Bad?
    /     \
 Good      Bad
  ↓         ↓
Search     Search
earlier    later
history    history
```

Each answer reduces the search space.

Eventually, Git identifies the first bad commit.

---

# First Bad Commit

Suppose the history is:

```text
C1 → Good
C2 → Good
C3 → Good
C4 → Bad
C5 → Bad
C6 → Bad
```

The first bad commit is:

```text
C4
```

This means the bug was introduced by commit `C4`.

---

# Manual Bisect Workflow

A basic manual workflow is:

```bash
git bisect start

git bisect bad

git bisect good <known-good-commit>
```

Git then selects a commit to test.

If the selected commit is bad:

```bash
git bisect bad
```

If it is good:

```bash
git bisect good
```

Continue until Git identifies the first bad commit.

---

# Automated Bisect

Manual testing is useful, but automation can make bisect much faster.

Suppose the project has a test script:

```text
test_pipeline.sh
```

The script should indicate whether the current commit is working or broken.

For example:

```text
0 → Test passed / Good
1 → Test failed / Bad
```

Then we can run:

```bash
git bisect run ./test_pipeline.sh
```

Git will automatically:

1. Check out a commit.
2. Run the test.
3. Determine whether it is good or bad.
4. Select another commit.
5. Repeat the process.

---

# Manual vs Automated Bisect

## Manual

```text
Git checks out commit
        ↓
You test it
        ↓
git bisect good/bad
        ↓
Next commit
```

## Automated

```text
Git checks out commit
        ↓
Test Script
        ↓
Exit Status
        ↓
Git determines Good/Bad
        ↓
Next commit
```

Automated bisect is especially useful when the project has a reliable automated test.

---

# Test Script Exit Status

For automated bisect, the test command must provide an appropriate exit status.

For example:

```text
0 → Good / Test Passed
1 → Bad / Test Failed
```

This allows Git to interpret the test result.

Example:

```bash
git bisect run python test_pipeline.py
```

---

# Finding the First Bad Commit

Suppose Git performs several tests:

```text
C25
 ↓
C37
 ↓
C43
 ↓
C41
 ↓
C42
```

Eventually Git identifies the first bad commit.

Once identified, inspect it using:

```bash
git show <commit-hash>
```

This can help you understand what changed in that commit.

---

# Inspecting the Bad Commit

After finding the problematic commit, use:

```bash
git show <commit-hash>
```

This can show:

- Commit author
- Commit message
- Changed files
- Code changes
- Commit details

You can then investigate why that particular change introduced the bug.

---

# Ending a Bisect Session

After identifying the first bad commit, exit Bisect Mode:

```bash
git bisect reset
```

This returns the repository to the original `HEAD` from before the bisect session.

---

# Complete Bisect Workflow

```text
git bisect start
        ↓
git bisect bad
        ↓
git bisect good <known-good-commit>
        ↓
Git selects a commit
        ↓
Test the commit
        ↓
   ┌────┴────┐
   ↓         ↓
 Good       Bad
   ↓         ↓
git bisect  git bisect
good        bad
   └────┬────┘
        ↓
Next Commit
        ↓
Repeat
        ↓
First Bad Commit
        ↓
git show <hash>
        ↓
git bisect reset
```

---

# When to Use `git bisect`

`git bisect` is particularly useful for finding **regression bugs**.

---

# What is a Regression?

A regression occurs when something that previously worked becomes broken after a later change.

Example:

```text
Commit A → Working ✅
Commit B → Working ✅
Commit C → Working ✅
Commit D → Broken ❌
```

Here, the bug was introduced somewhere between the known good and bad commits.

---

# Data Project Example

Suppose a data pipeline previously worked correctly:

```text
CSV
 ↓
ETL
 ↓
Database
```

Later, the pipeline starts producing incorrect results.

There may be hundreds of commits between the working version and the current version.

Instead of checking every commit:

```bash
git bisect start
```

can be used to efficiently narrow down the problematic commit.

---

# When Bisect Works Best

Bisect is most effective when you can easily determine whether a particular commit is:

```text
Good ✅
```

or:

```text
Bad ❌
```

The easier it is to test each version, the more useful bisect becomes.

---

# Automated Testing Makes Bisect More Powerful

Suppose we have:

```bash
python test_pipeline.py
```

The test:

```text
passes → exit code 0
fails  → exit code 1
```

Then:

```bash
git bisect run python test_pipeline.py
```

can automatically search the commit history.

---

# Complete Example

Suppose the history is:

```text
A ── B ── C ── D ── E ── F ── G
GOOD                         BAD
```

The current commit `G` contains the bug.

---

## Step 1 — Start Bisect

```bash
git bisect start
```

---

## Step 2 — Mark Current Commit as Bad

```bash
git bisect bad
```

---

## Step 3 — Mark the Last Known Good Commit

Suppose `A` was the last known good commit:

```bash
git bisect good A
```

Git now selects a commit around the middle.

Suppose it selects `D`.

---

## Step 4 — Test `D`

If `D` works correctly:

```bash
git bisect good
```

Git knows the first bad commit must come after `D`.

---

## Step 5 — Test the Next Selected Commit

Suppose Git selects `F`.

If `F` contains the bug:

```bash
git bisect bad
```

The search range becomes smaller.

---

## Step 6 — Continue

Git keeps narrowing the search until the first bad commit is identified.

Suppose the result is:

```text
E = Good
F = Bad
```

Then:

```text
F = First Bad Commit
```

---

## Step 7 — Inspect the Commit

```bash
git show F
```

Then investigate the changes introduced by that commit.

---

## Step 8 — Exit Bisect Mode

```bash
git bisect reset
```

---

# Important Commands

| Command | Purpose |
|---------|---------|
| `git bisect start` | Start a bisect session |
| `git bisect bad` | Mark the current commit as bad |
| `git bisect good <hash>` | Mark a known commit as good |
| `git bisect good` | Mark the current commit as good |
| `git bisect bad` | Mark the current commit as bad |
| `git bisect run <script>` | Automate the search |
| `git show <hash>` | Inspect a commit |
| `git bisect reset` | Exit bisect mode |

---

# Common Beginner Mistakes

- Starting bisect without identifying a known good commit.
- Marking a commit as good or bad without properly testing it.
- Forgetting to exit bisect mode.
- Using an unreliable automated test.
- Using a test script with incorrect exit codes.
- Assuming the first bad commit is necessarily the commit that directly contains the bug's visible symptom.

---

# Best Practices

- Choose a reliable known-good commit.
- Make sure the current bad commit is genuinely reproducible.
- Use automated tests when possible.
- Ensure automated test exit codes are correct.
- Inspect the identified commit with `git show`.
- Always finish the session with `git bisect reset`.

---

# Summary

`git bisect` is a debugging tool that uses binary search to identify the first bad commit in a project's history.

The process begins by identifying a known good commit and a known bad commit. Git then checks commits between them and repeatedly narrows the search space based on whether each tested commit is good or bad.

For automated testing, `git bisect run` can perform the process automatically.

Once the first bad commit is identified, `git show` can be used to inspect its changes. After the investigation, `git bisect reset` exits the bisect session.

---

# Key Takeaways

- `git bisect` uses binary search through commit history.
- It is useful for finding regression bugs.
- The goal is to identify the **First Bad Commit**.
- `git bisect good` marks a working commit.
- `git bisect bad` marks a broken commit.
- `git bisect run` can automate testing.
- `git show` helps inspect the identified commit.
- `git bisect reset` exits Bisect Mode.
- Binary search makes finding a problematic commit much faster than checking commits one by one.

---

# Practice Commands

## Start Bisect

```bash
git bisect start
```

## Mark Current Commit as Bad

```bash
git bisect bad
```

## Mark Known Good Commit

```bash
git bisect good <commit-hash>
```

## Mark Current Commit as Good

```bash
git bisect good
```

## Mark Current Commit as Bad

```bash
git bisect bad
```

## Automated Bisect

```bash
git bisect run <script>
```

Example:

```bash
git bisect run python test_pipeline.py
```

## Inspect the First Bad Commit

```bash
git show <commit-hash>
```

## Exit Bisect Mode

```bash
git bisect reset
```

---

# Exercises

- [ ] Create a repository with several commits.
- [ ] Introduce a bug in one of the commits.
- [ ] Use `git bisect` to find the first bad commit.
- [ ] Practice marking commits as `good` and `bad`.
- [ ] Inspect the identified commit using `git show`.
- [ ] Practice exiting with `git bisect reset`.
- [ ] Create a simple automated test and use `git bisect run`.

---

# What I Learned

After studying this chapter, I learned:

- What Git Bisect is.
- How binary search can be used for debugging.
- How to identify a known good and bad commit.
- How Git narrows down the search space.
- How to manually perform a bisect session.
- How to automate bisect with a test script.
- How to identify the first bad commit.
- How to inspect the problematic commit.
- How to exit Bisect Mode safely.