

````markdown
# Git Large File Storage (Git LFS)

## Overview

**Git LFS (Git Large File Storage)** is an extension of Git designed for managing large files efficiently.

Git works especially well with source code and small text files. However, projects such as Data Science, Machine Learning, multimedia, and large datasets may contain files that are much larger, such as:

- Datasets
- Machine learning models
- `.csv` files
- `.zip` files
- `.mp4` files
- Large binary files

Git LFS allows these large files to be managed separately from the normal Git object storage.

> **Git LFS stores a small pointer file in the Git repository while the actual large file is stored in LFS storage.**

---

# Normal Git vs Git LFS

Suppose a project contains:

```text
rice_disease_dataset.csv
````

with a size of:

```text
500 MB
```

## Normal Git

```text
Git Repository
      │
      ▼
500 MB CSV
```

The actual file and its history are managed by Git.

For large files that change frequently, this can make the repository unnecessarily large.

---

## Git LFS

With Git LFS:

```text
Git Repository
      │
      ▼
Small Pointer File
      │
      ▼
LFS Storage
      │
      ▼
500 MB Actual File
```

Therefore:

```text
Git Repository → Pointer
LFS Storage    → Actual Large File
```

This is the core idea behind Git LFS.

---

# What is a Pointer File?

When a file is tracked using Git LFS, the actual file content is not stored directly in the normal Git repository.

Instead, Git stores a small **pointer file**.

Conceptually, it may look like:

```text
version https://git-lfs.github.com/spec/v1
oid sha256:xxxxxxxxxxxxxxxx
size 524288000
```

This is **not the actual file**.

The pointer tells Git LFS where the actual content belongs and identifies it using information such as its object ID and size.

---

# Why is Git LFS Useful?

Git LFS is particularly useful for Data Science and Machine Learning projects.

For example:

```text
rice-disease-classification/
│
├── notebooks/
├── src/
├── models/
│   └── best_model.h5
└── data/
    └── train.csv
```

Suppose:

```text
best_model.h5 → 300 MB
train.csv     → 500 MB
```

Instead of storing these large files directly in the normal Git object history:

```text
Git
├── source code
├── notebooks
├── pointer files
└── project history

LFS
├── best_model.h5
└── train.csv
```

---

# Installing and Initializing Git LFS

Git LFS needs to be installed on the system before it can be used.

After installation, initialize it with:

```bash
git lfs install
```

This enables Git LFS for your Git environment.

Usually, this initialization only needs to be performed once per environment.

---

# Tracking Large Files

Suppose you want Git LFS to track all CSV files.

Run:

```bash
git lfs track "*.csv"
```

This tells Git LFS:

```text
*.csv → Git LFS
```

From this point, matching CSV files are configured to use Git LFS.

---

# The `.gitattributes` File

When you run:

```bash
git lfs track "*.csv"
```

Git LFS creates or updates:

```text
.gitattributes
```

It contains rules that tell Git how to handle the selected files.

Conceptually, it may contain:

```text
*.csv filter=lfs diff=lfs merge=lfs -text
```

The purpose is to tell Git:

> Files matching this pattern should be handled by Git LFS.

---

# Git LFS Workflow

Once LFS has been configured, the workflow is similar to normal Git.

```bash
git add .gitattributes
git add train.csv

git commit -m "Track dataset with Git LFS"

git push
```

The important difference is that the LFS-tracked file's actual content is stored through LFS rather than directly in normal Git object storage.

---

# Complete Git LFS Workflow

Suppose you want to track:

```text
train.csv
```

using Git LFS.

## Step 1 — Initialize Git LFS

```bash
git lfs install
```

## Step 2 — Track the File

```bash
git lfs track "*.csv"
```

## Step 3 — Stage the Configuration and File

```bash
git add .gitattributes
git add train.csv
```

## Step 4 — Commit

```bash
git commit -m "Add dataset using Git LFS"
```

## Step 5 — Push

```bash
git push
```

The overall flow is:

```text
Large File
    │
    ▼
git lfs track
    │
    ▼
.gitattributes
    │
    ▼
git add
    │
    ▼
git commit
    │
    ▼
git push
    │
    ├──────────────► Git Repository
    │                  │
    │                  ▼
    │               Pointer
    │
    └──────────────► LFS Storage
                       │
                       ▼
                 Actual File
```

---

# Updating an LFS File

Suppose:

```text
train.csv
```

is already tracked by Git LFS.

After updating the dataset, the normal Git workflow can be used:

```bash
git add train.csv

git commit -m "Update training dataset"

git push
```

Git LFS handles the large file content through the LFS mechanism.

---

# Downloading LFS Files

When working with a repository containing LFS-tracked files, you may need to retrieve the actual LFS objects.

Use:

```bash
git lfs pull
```

Conceptually:

```text
Git Repository
      │
      ▼
Pointer File
      │
      ▼
git lfs pull
      │
      ▼
LFS Storage
      │
      ▼
Actual Large File
```

---

# `git pull` vs `git lfs pull`

These commands serve different purposes.

## `git pull`

```bash
git pull
```

Retrieves changes from the normal Git remote repository.

---

## `git lfs pull`

```bash
git lfs pull
```

Retrieves or updates the actual LFS-tracked file content.

In a properly configured Git LFS environment, LFS content may also be handled as part of the normal clone/pull workflow. `git lfs pull` is useful when you specifically want to ensure the LFS objects are available locally.

---

# Listing LFS-Tracked Files

To see which files are currently tracked by Git LFS:

```bash
git lfs ls-files
```

Example:

```text
a83f21d train.csv
c91ab52 model.h5
```

This provides a quick way to inspect the files managed by LFS.

---

# What Types of Files Work Well with Git LFS?

Git LFS is commonly useful for large files such as:

## Datasets

```text
.csv
.parquet
.zip
```

## Machine Learning Models

```text
.h5
.pt
.pth
.onnx
.pkl
```

## Large Binary or Media Files

```text
.psd
.mp4
large images
```

The important factor is not simply the file extension, but whether the file is large and appropriate for large-file storage.

---

# When Should You Use Git LFS?

Git LFS can be useful when:

## 1. The File is Large

For example:

```text
500 MB dataset
```

---

## 2. The File Changes Over Time

For example:

```text
model_v1
model_v2
model_v3
```

---

## 3. You Need Versioning

For example:

```text
Dataset v1
Dataset v2
Dataset v3
```

Git LFS allows these files to participate in a Git-based versioning workflow.

---

## 4. A Team Needs Shared Versions

A team working on the same project can use Git LFS to manage large tracked files alongside the Git repository.

---

# When Should You Avoid Git LFS?

Not every file needs Git LFS.

Small text files such as:

```text
main.py
README.md
config.yaml
requirements.txt
```

are normally better suited to regular Git.

A simple mental model is:

```text
Source Code
     ↓
Regular Git

Large Files
     ↓
Git LFS
```

---

# Limitations of Git LFS

Git LFS is useful, but it also introduces some considerations.

## Storage and Bandwidth

LFS hosting may have storage and bandwidth limits depending on the hosting provider and plan.

Large datasets and models can consume significant resources.

---

## Large Transfers Can Be Slow

Very large files can take significant time to upload or download.

For example:

```text
5 GB
10 GB
50 GB
```

Moving files of this size requires substantial bandwidth and storage.

---

## Additional Tooling

Team members need to understand:

```text
Git
+
Git LFS
```

Therefore, Git LFS introduces an additional part to the project workflow.

---

# Git LFS vs Git Submodule

Git LFS and Git Submodules solve completely different problems.

| Feature              | Git Submodule                | Git LFS                        |
| -------------------- | ---------------------------- | ------------------------------ |
| Main purpose         | Include another repository   | Manage large files             |
| Manages              | Git repositories             | Large files                    |
| Separate Git history | Yes                          | No separate repository history |
| Pointer/reference    | Submodule commit reference   | LFS pointer                    |
| Dataset management   | Usually not the main purpose | Yes                            |
| External repository  | Yes                          | No                             |

The easiest way to remember the difference:

```text
Git Submodule

Repository
    │
    ▼
Another Repository
```

Whereas:

```text
Git LFS

Repository
    │
    ▼
Pointer
    │
    ▼
Large File Storage
```

---

# Git LFS Architecture

The core concept can be represented as:

```text
                 Git Repository
                       │
             ┌─────────┴─────────┐
             │                   │
          Code              LFS Pointer
                                  │
                                  ▼
                            LFS Storage
                                  │
                                  ▼
                           Actual Large File
```

For example:

```text
Git Repository
│
├── train.py
├── README.md
├── .gitattributes
└── train.csv → LFS Pointer
                       │
                       ▼
                  LFS Storage
                       │
                       ▼
                  Actual train.csv
```

---

# Best Practices

## Track Only Appropriate Large Files

Avoid blindly tracking everything.

Instead of:

```bash
git lfs track "*"
```

prefer targeted patterns such as:

```bash
git lfs track "*.csv"
git lfs track "*.h5"
```

Only track files that actually need LFS.

---

## Document LFS Usage

If your project uses Git LFS, mention it in the README.

For example:

```text
This project uses Git LFS.
Please install Git LFS before working with the repository.
```

---

## Monitor Storage Usage

Large datasets and models can consume significant storage and bandwidth.

Keep track of the project's LFS usage, especially when working with large or frequently updated files.

---

# Data Science Project Example

Suppose you have a project:

```text
retail-intelligence-platform/
│
├── data/
│   ├── customers.csv
│   ├── sales.csv
│   └── products.csv
│
├── notebooks/
├── src/
├── models/
│   └── model.pkl
│
└── README.md
```

Suppose:

```text
sales.csv
```

is large enough to require LFS.

Track it:

```bash
git lfs install
git lfs track "data/sales.csv"
```

Then:

```bash
git add .gitattributes data/sales.csv

git commit -m "Track large sales dataset with Git LFS"

git push
```

The project conceptually becomes:

```text
Git Repository
│
├── src/
├── notebooks/
├── README.md
├── .gitattributes
└── sales.csv → LFS Pointer
                     │
                     ▼
                LFS Storage
                     │
                     ▼
                Actual sales.csv
```

---

# Essential Commands

| Command                   | Purpose                       |
| ------------------------- | ----------------------------- |
| `git lfs install`         | Initialize Git LFS            |
| `git lfs track "*.csv"`   | Track matching files with LFS |
| `git lfs ls-files`        | List LFS-tracked files        |
| `git lfs pull`            | Download/update LFS content   |
| `git add .gitattributes`  | Stage LFS tracking rules      |
| `git add <file>`          | Stage an LFS-tracked file     |
| `git commit -m "message"` | Commit changes                |
| `git push`                | Push Git and LFS changes      |

---

# Quick Mental Model

Remember Git LFS using this simple diagram:

```text
Small Code/Text Files
        │
        ▼
   Git Repository


Large Files
    │
    ▼
LFS Pointer
    │
    ▼
LFS Storage
    │
    ▼
Actual Large File
```

> **Git LFS manages large files by keeping a small pointer in the Git repository while storing the actual file through LFS storage.**

---

# Exam Note

> **Git LFS (Git Large File Storage) is a Git extension used to manage large files such as datasets, machine learning models, and binary files. Instead of storing the actual large file directly in the normal Git repository, Git stores a small pointer while the actual file is stored in LFS storage.**

---

# Key Takeaways

* **LFS** stands for **Git Large File Storage**.
* Git LFS is designed for large files such as datasets and ML models.
* The Git repository contains a small LFS pointer instead of the actual large file content.
* `.gitattributes` defines which files are handled by LFS.
* `git lfs install` initializes LFS.
* `git lfs track` specifies which files should use LFS.
* `git lfs ls-files` lists LFS-tracked files.
* `git lfs pull` retrieves LFS content.
* Git LFS and Git Submodules solve different problems.

---

# Practice Commands

```bash
git lfs install

git lfs track "*.csv"

git lfs ls-files

git add .gitattributes

git add train.csv

git commit -m "Track dataset with Git LFS"

git push

git lfs pull
```

---

# Exercises

* [ ] Install and initialize Git LFS.
* [ ] Track a `.csv` file using Git LFS.
* [ ] Inspect the generated `.gitattributes` file.
* [ ] List LFS-tracked files.
* [ ] Commit an LFS-tracked file.
* [ ] Push the repository.
* [ ] Practice retrieving LFS content using `git lfs pull`.
* [ ] Explain the difference between Git LFS and Git Submodules.

---

# What I Learned

After studying Git LFS, I learned:

* Why large files can be problematic for normal Git workflows.
* How Git LFS separates large file storage from normal Git repository data.
* What an LFS pointer file is.
* How `.gitattributes` controls LFS tracking.
* How to track datasets and machine learning models using Git LFS.
* How Git LFS differs from Git Submodules.

````


