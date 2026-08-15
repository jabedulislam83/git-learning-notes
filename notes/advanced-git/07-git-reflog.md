````markdown
# Advanced Git — Git Reflog

## 1. What is Git Reflog?

`git reflog` is a **local recovery tool** that records where Git references such as `HEAD` and branches have pointed over time.

In simple terms:

> **`git reflog` = Git's local "time machine".**

It can help recover from mistakes such as:

- Accidental `git reset --hard`
- Incorrect `git rebase`
- Deleted branches
- Moving `HEAD` to the wrong commit
- Accidentally changing commit history

For example:

```text
HEAD → Commit C
````

Then you run:

```bash
git reset --hard HEAD~2
```

Now:

```text
Before:
HEAD → C

After:
HEAD → A
```

The previous position of `HEAD` may still be recorded in the reflog.

So Git can often tell you:

> "You were here before."

---

# 2. `git log` vs `git reflog`

This distinction is very important.

## `git log`

`git log` mainly shows the **reachable commit history**.

For example:

```text
A → B → C → D
```

If the current branch points to `D`:

```text
D
↓
C
↓
B
↓
A
```

You can view this history using:

```bash
git log
```

---

## `git reflog`

```bash
git reflog
```

This shows:

> **Where `HEAD` or another reference has moved over time.**

Suppose:

```text
A → B → C → D
```

You reset from `D` to `A`:

```bash
git reset --hard A
```

Now `C` and `D` may no longer be reachable from the current branch.

`git log` may no longer show them.

But:

```bash
git reflog
```

may show:

```text
HEAD@{0} → A
HEAD@{1} → D
HEAD@{2} → C
```

This is the main power of `reflog`.

---

# 3. Simple Example

Suppose your history is:

```text
A → B → C → D
```

Currently:

```text
HEAD → D
```

Then you accidentally run:

```bash
git reset --hard B
```

Now:

```text
A → B → C → D
    ↑
   HEAD
```

You may think:

> "My work is gone!"

Not necessarily.

Run:

```bash
git reflog
```

You may see:

```text
abc1234 HEAD@{0}: reset: moving to B
def5678 HEAD@{1}: commit: Add feature
ghi9012 HEAD@{2}: commit: Update model
```

From the reflog, you can identify where `HEAD` was before the reset.

---

# 4. Basic `git reflog` Command

The simplest command is:

```bash
git reflog
```

You can also use:

```bash
git reflog show
```

Both can be used to inspect the reflog.

Example output:

```text
d789012 HEAD@{0}: reset: moving to HEAD~2
c345678 HEAD@{1}: commit: Add validation
b123456 HEAD@{2}: commit: Update pipeline
a111111 HEAD@{3}: checkout: moving from main to etl-feature
```

---

# 5. How to Read Reflog Output

Consider this line:

```text
d789012 HEAD@{0}: reset: moving to HEAD~2
```

There are several important parts.

## Commit Hash

```text
d789012
```

This is the short commit hash.

---

## `HEAD@{0}`

```text
HEAD@{0}
```

This represents the latest reflog entry.

Generally:

```text
HEAD@{0} → Most recent
HEAD@{1} → Previous
HEAD@{2} → Older
HEAD@{3} → Even older
```

---

## Operation

```text
reset
```

This tells you what operation occurred.

Common examples include:

```text
commit
reset
merge
checkout
rebase
```

---

## Description

```text
moving to HEAD~2
```

This describes what the operation did.

---

# 6. What is `HEAD@{n}`?

This is an important concept.

```text
HEAD@{0}
```

means:

> The current/latest reflog position.

```text
HEAD@{1}
```

means:

> The previous reflog position.

```text
HEAD@{2}
```

means:

> The position before that.

For example:

```text
HEAD@{0} → Commit D
HEAD@{1} → Commit C
HEAD@{2} → Commit B
HEAD@{3} → Commit A
```

This allows you to identify previous states of `HEAD`.

---

# 7. Recovering a Deleted Branch

This is one of the most useful practical applications of `git reflog`.

Suppose you have a branch:

```text
etl-feature
```

It contains important work.

You accidentally delete it:

```bash
git branch -D etl-feature
```

The branch reference is removed.

However, the commits may still exist in your local Git repository.

## Step 1 — Check the Reflog

Run:

```bash
git reflog
```

You might find something like:

```text
abc1234 HEAD@{4}: checkout: moving from etl-feature to main
```

The commit:

```text
abc1234
```

may be the last commit of the deleted branch.

---

## Step 2 — Inspect the Commit

You can inspect it using:

```bash
git show abc1234
```

---

## Step 3 — Create a New Branch

Using modern Git:

```bash
git switch -c etl-feature abc1234
```

Or using the older syntax:

```bash
git checkout -b etl-feature abc1234
```

Now:

```text
Deleted Branch
      ↓
Find Commit Using Reflog
      ↓
Create New Branch
      ↓
Branch Recovered ✅
```

---

# 8. `git reset` and `reflog`

Recall that Git has different types of reset.

## Soft Reset

```bash
git reset --soft HEAD~1
```

* Moves `HEAD`
* Keeps changes staged
* Does not modify the working directory

---

## Mixed Reset

```bash
git reset --mixed HEAD~1
```

Or simply:

```bash
git reset HEAD~1
```

It:

* Moves `HEAD`
* Unstages changes
* Keeps working-directory changes

---

## Hard Reset

```bash
git reset --hard HEAD~1
```

It:

* Moves `HEAD`
* Changes the staging area
* Discards working-directory changes

Because `--hard` can discard work, it is especially important to know about `git reflog`.

---

# 9. What Happens After `reset --hard`?

Suppose:

```text
A → B → C → D
```

You are currently at `D`.

Then you run:

```bash
git reset --hard B
```

Now:

```text
A → B
    ↑
   HEAD
```

You may think:

> "C and D are gone!"

But first check:

```bash
git reflog
```

You may see:

```text
HEAD@{0}: reset: moving to B
HEAD@{1}: commit: D
HEAD@{2}: commit: C
```

This means the previous commits may still be recoverable.

---

# 10. Recovering a Lost Commit

Suppose the reflog contains:

```text
abc1234 HEAD@{1}: commit: Add new feature
```

You can move back to that state with:

```bash
git reset --soft HEAD@{1}
```

Or use the commit hash directly:

```bash
git reset --soft abc1234
```

The `--soft` option keeps the changes staged.

---

# 11. Why Use `git reset --soft HEAD@{1}`?

Suppose:

```text
Current HEAD
     ↓
     A

Previous HEAD
     ↓
     B
```

You want:

```text
HEAD → B
```

but you do not want to lose the changes introduced after `B`.

So you can use:

```bash
git reset --soft HEAD@{1}
```

Because `--soft` is used, the changes remain staged.

---

# 12. Filtering Reflog by Time

You can view reflog entries from a specific time period.

For example:

```bash
git reflog --since="1 week ago"
```

This shows reflog entries from approximately the last week.

Other examples:

```bash
git reflog --since="2 days ago"
```

Or:

```bash
git reflog --until="1 week ago"
```

---

# 13. Important Limitation

`reflog` is primarily a **local repository feature**.

It is not the permanent history of GitHub or another remote repository.

Conceptually:

```text
Your Local Git
     ↓
   Reflog ✅

Remote Repository
     ↓
   Your local reflog is not available there ❌
```

You generally cannot inspect another developer's local reflog.

---

# 14. Reflog Is Not Permanent

Reflog entries can expire over time.

Therefore:

> **If you make a Git mistake, use `git reflog` as soon as possible.**

Git can eventually prune old unreachable objects and reflog entries.

You may see commands related to reflog maintenance, such as:

```bash
git reflog expire
```

This is an advanced maintenance operation and normally does not need to be run manually during everyday Git usage.

---

# 15. `git log` vs `git reflog` — Quick Comparison

| Feature                         | `git log`          | `git reflog`           |
| ------------------------------- | ------------------ | ---------------------- |
| Shows                           | Commit history     | Reference movement     |
| Scope                           | Repository history | Local repository       |
| Shows previous `HEAD` positions | Usually ❌          | ✅                      |
| Helps recover deleted branches  | Limited            | ✅                      |
| Helps recover after reset       | Limited            | ✅                      |
| Shows unreachable commits       | Usually ❌          | Often ✅                |
| Permanent                       | Commit history     | Entries can expire     |
| Main purpose                    | View history       | Recovery and debugging |

---

# 16. When Should You Use `git reflog`?

Use `git reflog` when:

## Accidentally Deleted a Branch

```bash
git branch -D etl-feature
```

Then:

```bash
git reflog
```

---

## Accidentally Used `reset --hard`

```bash
git reset --hard HEAD~3
```

Then:

```bash
git reflog
```

---

## Made a Mistake During Rebase

```bash
git rebase ...
```

Then:

```bash
git reflog
```

---

## Accidentally Changed Commit History

If you cannot find an old commit:

```bash
git reflog
```

---

## Need to Find an Earlier Working State

```bash
git reflog
```

---

# 17. Complete Recovery Example

Suppose:

```text
A → B → C → D
```

You are currently at `D`.

You accidentally run:

```bash
git reset --hard B
```

Now the branch is:

```text
A → B
    ↑
   HEAD
```

## Step 1 — Check Reflog

```bash
git reflog
```

Example:

```text
b111111 HEAD@{0}: reset: moving to B
d444444 HEAD@{1}: commit: Add final feature
c333333 HEAD@{2}: commit: Fix validation
```

## Step 2 — Identify the Previous State

The previous working state was:

```text
HEAD@{1}
```

## Step 3 — Recover It

You can run:

```bash
git reset --hard HEAD@{1}
```

Now:

```text
A → B → C → D
            ↑
           HEAD
```

Your previous state has been restored. ✅

---

# 18. Most Important Concept

Remember `git reflog` like this:

```text
git log
   ↓
"What is my repository history?"

git reflog
   ↓
"Where has my HEAD/branch been?"
```

A useful mental model:

```text
                 Git Reflog
                     ↓
          ┌────────────────────┐
          │  Reference History │
          └────────────────────┘
             ↓       ↓       ↓
           reset   rebase   delete
             ↓       ↓       ↓
             └───────┴───────┘
                     ↓
                RECOVERY 🔄
```

---

# 19. Five Important Commands to Remember

```bash
git reflog
```

```bash
git reflog show
```

```bash
git reflog --since="1 week ago"
```

```bash
git reset --soft HEAD@{1}
```

```bash
git switch -c recovered-branch <commit-hash>
```

---

# One-Line Definition

> **`git reflog` is Git's local recovery history that records where `HEAD` and branches have pointed, helping recover work after mistakes such as `reset`, `rebase`, or branch deletion.**

---

# Exercises

* [ ] Create a practice repository.
* [ ] Create several commits.
* [ ] Run `git reflog` and inspect the output.
* [ ] Practice `git reset --soft`.
* [ ] Practice `git reset --mixed`.
* [ ] Practice `git reset --hard` in a test repository.
* [ ] Use `git reflog` to recover from a hard reset.
* [ ] Create a branch and delete it.
* [ ] Use `git reflog` to find the deleted branch's last commit.
* [ ] Recover the deleted branch.
* [ ] Practice viewing reflog entries using `--since`.
* [ ] Practice recovering an earlier working state.

---

# What I Learned

After studying this chapter, I learned:

* What `git reflog` is.
* Why `git reflog` is useful for Git recovery.
* The difference between `git log` and `git reflog`.
* What `HEAD@{n}` means.
* How to read reflog output.
* How to recover a deleted branch.
* How to recover from an accidental `git reset --hard`.
* How `git reflog` can help after a problematic rebase.
* How to use a reflog entry with `git reset`.
* How to filter reflog entries by time.
* Why reflog is primarily local.
* Why reflog entries can expire.
* When to use `git reflog` for recovery and debugging.

```
```
