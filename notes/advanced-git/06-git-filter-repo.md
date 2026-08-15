```markdown
# Advanced Git — Git Filter-Repo

## 1. What is Git Filter-Repo?

`git filter-repo` is a tool used to **rewrite, modify, or clean the history of a Git repository**.

In simple terms:

> **Git Filter-Repo = A tool for rewriting old Git commit history.**

It can be used to:

- Remove sensitive files from Git history
- Remove API keys or passwords from history
- Remove large unnecessary files
- Remove unwanted directories
- Rename directories in repository history
- Clean and restructure repository history

`git filter-repo` is generally faster, more reliable, and easier to use than the older `git filter-branch` approach.

---

# 2. Why Do We Need Git Filter-Repo?

Suppose you accidentally committed a file:

~~~text
secrets.txt
~~~

The file contains:

~~~text
API_KEY=abc123
DB_PASSWORD=mypassword
~~~

You might try to simply delete the file:

~~~bash
rm secrets.txt
git add .
git commit -m "Remove secrets"
~~~

However, this does **not completely solve the problem**.

Why?

Because the file still exists in older commits.

For example:

~~~text
Commit 1 → secrets.txt added
Commit 2 → secrets.txt modified
Commit 3 → secrets.txt deleted
~~~

The current version no longer contains the file:

~~~text
Current Working Directory
        ↓
secrets.txt does not exist
~~~

But the Git history may still contain it:

~~~text
Git History
    ↓
secrets.txt still exists
~~~

This is where `git filter-repo` can be useful.

---

# 3. Installing Git Filter-Repo

First, install `git-filter-repo` using:

~~~bash
pip install git-filter-repo
~~~

You can check whether it is available with:

~~~bash
git filter-repo --help
~~~

Or:

~~~bash
git filter-repo --version
~~~

---

# 4. Removing a File from the Entire History

Suppose the file is:

~~~text
secrets.txt
~~~

To remove it from the repository history:

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

There are two important parts here.

## `--path`

~~~bash
--path secrets.txt
~~~

This specifies the path we want to target.

In this case:

~~~text
secrets.txt
~~~

## `--invert-paths`

~~~bash
--invert-paths
~~~

This means:

> Remove the specified path and keep everything else.

Conceptually:

~~~text
secrets.txt → Remove

Other files → Keep
~~~

Therefore:

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

means:

> Rewrite the repository history while excluding `secrets.txt`.

---

# 5. An Important Point — Commit Hashes Change

`git filter-repo` rewrites Git history.

Suppose the original history is:

~~~text
A → B → C → D
~~~

After rewriting the history, it may become:

~~~text
A' → B' → C' → D'
~~~

The rewritten commits can have different hashes:

~~~text
A ≠ A'
B ≠ B'
C ≠ C'
D ≠ D'
~~~

Therefore:

> **Rewriting Git history can change commit hashes.**

---

# 6. Why Do Commit Hashes Change?

A Git commit contains information about its parent commit.

Suppose we have:

~~~text
A → B → C
~~~

Commit `C` contains a reference to `B` as its parent.

If `B` is rewritten:

~~~text
A → B'
~~~

then the original `C` can no longer reference the rewritten `B` in the same way.

Therefore, `C` may also need to be rewritten:

~~~text
A → B' → C'
~~~

This is why rewriting one part of Git history can cause later commit hashes to change as well.

---

# 7. What Happens After Running Filter-Repo?

Suppose the original history contains:

~~~text
Commit 1
 ├── app.py
 └── secrets.txt

Commit 2
 ├── app.py
 └── secrets.txt

Commit 3
 └── app.py
~~~

Now run:

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

Conceptually, the rewritten history becomes:

~~~text
Commit 1'
 └── app.py

Commit 2'
 └── app.py

Commit 3'
 └── app.py
~~~

The `secrets.txt` path has been removed from the rewritten history.

---

# 8. An Important Limitation

Removing a file does not automatically fix references to that file inside other files.

Suppose:

~~~text
config.txt
~~~

contains:

~~~text
secret_file = secrets.txt
~~~

Now run:

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

The `secrets.txt` path can be removed from the history.

However, the following line inside `config.txt`:

~~~text
secret_file = secrets.txt
~~~

may still remain.

Therefore, references to the removed file may need to be updated manually.

---

# 9. Using Filter-Repo with a Remote Repository

Suppose the repository is hosted on GitHub:

~~~text
Local Repository
       │
       ▼
     GitHub
~~~

After rewriting the local history:

~~~text
Old History
    ↓
filter-repo
    ↓
New History
~~~

The local and remote histories may no longer match:

~~~text
Local
 ↓
New History

GitHub
 ↓
Old History
~~~

Updating the remote may therefore require a force push.

For example:

~~~bash
git push --force
~~~

Or a safer option:

~~~bash
git push --force-with-lease
~~~

---

# 10. `--force` vs `--force-with-lease`

After rewriting history, you may need:

~~~bash
git push --force
~~~

However, this can overwrite remote history without checking for other changes.

A safer option is:

~~~bash
git push --force-with-lease
~~~

It checks whether the remote branch has changed unexpectedly before performing the force update.

Even with `--force-with-lease`:

> **Force pushing to a shared repository should be done carefully and only after coordinating with collaborators.**

---

# 11. Why Might Collaborators Need a Fresh Clone?

Suppose another developer has:

~~~text
Developer A
    ↓
Old History
~~~

You rewrite the repository history and push the new history:

~~~text
GitHub
    ↓
New History
~~~

Now the histories no longer match:

~~~text
Developer A
    ↓
Old History

GitHub
    ↓
New History
~~~

This can cause synchronization problems.

In some cases, the easiest solution is for collaborators to clone the repository again:

~~~bash
git clone <repository-url>
~~~

This gives them a fresh copy of the rewritten history.

---

# 12. When Should We Use `git filter-repo`?

`git filter-repo` is mainly useful when we need to **rewrite or clean old Git history**.

Common use cases include:

## Sensitive Information

~~~text
API Keys
Passwords
Credentials
Private Keys
Tokens
~~~

## Unnecessary Files

~~~text
Large Binary Files
Generated Files
Temporary Files
Unwanted Files
~~~

## Repository Restructuring

~~~text
Directory Rename
Remove Unwanted Paths
Clean Repository History
~~~

---

# 13. `git rm` vs `git filter-repo`

This distinction is very important.

## Using `git rm`

Suppose we run:

~~~bash
git rm secrets.txt
git commit -m "Remove secrets"
~~~

The file is removed from the current version:

~~~text
Current Version
    ↓
secrets.txt does not exist
~~~

But it may still exist in older history:

~~~text
Old History
    ↓
secrets.txt may still exist
~~~

---

## Using `git filter-repo`

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

The goal is to remove the specified path from the rewritten repository history:

~~~text
Current Version
    ↓
secrets.txt removed

Old History
    ↓
secrets.txt history removed
~~~

---

# 14. What If a Credential Was Leaked?

Suppose an API key was accidentally committed:

~~~text
API_KEY=abc123
~~~

Running `git filter-repo` is not enough by itself.

The credential may already have been exposed.

Therefore:

~~~text
Remove from Git History
        +
Revoke Credential
        +
Generate / Rotate New Credential
~~~

For example:

~~~text
Old API Key
    ↓
Revoke

New API Key
    ↓
Use
~~~

This is an important security practice.

---

# 15. Complete Workflow

Suppose a sensitive file was accidentally committed.

A typical workflow is:

~~~text
Sensitive File Accidentally Committed
                ↓
Install git-filter-repo
                ↓
Backup Repository
                ↓
Run filter-repo
                ↓
History Rewritten
                ↓
Commit Hashes Changed
                ↓
Verify History
                ↓
Revoke / Rotate Credentials
                ↓
Force Push Remote
                ↓
Inform Collaborators
                ↓
Fresh Clone if Necessary
~~~

---

# 16. Basic Example

Suppose we have:

~~~text
secrets.txt
~~~

and want to remove it from the repository history.

## Step 1 — Navigate to the Repository

~~~bash
cd my-project
~~~

## Step 2 — Run Filter-Repo

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

## Step 3 — Verify the History

~~~bash
git log --all -- secrets.txt
~~~

If the command returns no commits, the path is no longer present in the rewritten history.

## Step 4 — Check Repository Status

~~~bash
git status
~~~

## Step 5 — Update the Remote Carefully

If the rewritten history needs to be pushed:

~~~bash
git push --force-with-lease
~~~

---

# 17. Removing a Directory from History

The `--path` option can also target a directory.

Suppose the directory is:

~~~text
temp/
~~~

To remove it from the rewritten history:

~~~bash
git filter-repo --path temp/ --invert-paths
~~~

Conceptually:

~~~text
temp/
   ↓
Target

--invert-paths
   ↓
Remove Target
~~~

---

# 18. Important Warning

`git filter-repo` is not a normal Git command for everyday file management.

It rewrites repository history:

~~~text
Repository History
       ↓
    Rewrite
~~~

Therefore, before using it:

~~~text
Backup
  ↓
Check Repository
  ↓
Coordinate with Team
  ↓
Run Filter-Repo
  ↓
Verify
  ↓
Update Remote Carefully
~~~

This workflow is recommended, especially for shared repositories.

---

# 19. Shared Repository Warning

If the repository is used only by you:

~~~text
Personal Repository
        ↓
Filter Repo
        ↓
Force Push
~~~

Coordination is relatively simple.

But if it is a shared repository:

~~~text
Team
 ↓
Shared Repository
~~~

you should inform the other developers before rewriting history.

Why?

Because force pushing rewritten history can cause problems for their local repositories.

---

# 20. Quick Revision

| Topic | Meaning |
|---|---|
| `git filter-repo` | Tool for rewriting Git history |
| `--path` | Specifies the target path |
| `--invert-paths` | Removes the target path and keeps the rest |
| History Rewrite | Recreates affected history |
| Commit Hash | May change |
| `git rm` | Normally removes a file from the current version |
| Force Push | May be required to update a remote with rewritten history |
| `--force-with-lease` | Safer force-push option |
| Collaborators | May need to resynchronize or fresh clone |
| Main Use | Remove sensitive or unwanted data from history |

---

# 21. Most Important Command

~~~bash
git filter-repo --path secrets.txt --invert-paths
~~~

Its main meaning is:

> **Rewrite the repository history while excluding `secrets.txt`.**

---

# 22. Important Commands

~~~bash
# Install git-filter-repo
pip install git-filter-repo

# Check version
git filter-repo --version

# Remove a file from history
git filter-repo --path secrets.txt --invert-paths

# Remove a directory from history
git filter-repo --path temp/ --invert-paths

# Check history
git log --all -- secrets.txt

# Push rewritten history
git push --force-with-lease

# Clone a fresh copy
git clone <repository-url>
~~~

---

# 23. Filter-Repo vs Other Git Commands

| Command | Main Purpose |
|---|---|
| `git rm` | Remove a file from the current version |
| `git revert` | Reverse the changes introduced by a commit |
| `git reset` | Move HEAD/branch position |
| `git rebase` | Reorganize commit history |
| `git filter-repo` | Rewrite and clean repository history |

---

# Exam Note

> **`git filter-repo`** is a tool for rewriting and cleaning Git repository history. It can be used to remove sensitive files, API keys, passwords, unnecessary files, or directories from repository history. For example, **`git filter-repo --path secrets.txt --invert-paths`** rewrites the repository history while excluding `secrets.txt`. Because the history is rewritten, commit hashes may change. If the repository has a remote, updating the rewritten history may require a force push, with **`git push --force-with-lease`** being a safer option than blindly using **`--force`**. Shared repositories should be handled carefully because history rewriting can affect collaborators. If credentials have been exposed, they should also be revoked or rotated rather than relying only on removing them from Git history.

---

# Easy Way to Remember

~~~text
git rm
   ↓
Remove from Current Version
~~~

~~~text
git filter-repo
   ↓
Rewrite Git History
~~~

~~~text
Sensitive File
      ↓
filter-repo
      ↓
History Rewrite
      ↓
Commit Hashes Change
      ↓
Verify
      ↓
Force Push if Required
~~~

### One-Line Definition

> **`git filter-repo` = A tool for rewriting Git history to clean or remove unwanted content from the repository's history.**

---

# Exercises

- [ ] Create a practice repository.
- [ ] Create a `secrets.txt` file and commit it.
- [ ] Modify the file and create additional commits.
- [ ] Delete the file normally.
- [ ] Use `git log --all -- secrets.txt` to inspect its history.
- [ ] Practice `git filter-repo --path secrets.txt --invert-paths`.
- [ ] Verify the rewritten history.
- [ ] Observe how commit hashes change.
- [ ] Practice removing a directory from history.
- [ ] Review `git push --force-with-lease` in a practice repository.

---

# What I Learned

After studying this chapter, I learned:

- What `git filter-repo` is.
- Why deleting a file normally does not remove it from Git history.
- How the `--path` option works.
- How the `--invert-paths` option works.
- How to remove a file from repository history.
- Why history rewriting can change commit hashes.
- How history rewriting affects remote repositories.
- Why a force push may be required.
- Why `--force-with-lease` is safer than blindly using `--force`.
- Why collaborators may need to resynchronize or clone the repository again.
- Why exposed credentials must be revoked or rotated.
- When to use `git filter-repo` instead of `git rm`.
```
