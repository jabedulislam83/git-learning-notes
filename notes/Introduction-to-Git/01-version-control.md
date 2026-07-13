# Version Control

## Overview

Version control is a system that records changes made to files over time. It allows developers to track modifications, compare different versions, collaborate with others, and restore previous versions when necessary.

Git is one of the most popular distributed version control systems used by software developers around the world.

---

# Why Version Control Matters

Imagine writing a report for several weeks.

```
report.docx
report_final.docx
report_final_v2.docx
report_final_latest.docx
report_final_latest_new.docx
```

After a few days, it becomes difficult to remember which file is the newest.

Version control solves this problem by storing every change in a structured history.

---

# Problems Without Version Control

Without version control:

- Previous versions may be lost.
- Collaboration becomes difficult.
- Mistakes are hard to recover.
- Tracking changes is almost impossible.
- Multiple copies of the same project create confusion.

---

# What is Version Control?

Version control is a tool that keeps a complete history of your project.

Instead of creating multiple copies of the same file, Git stores every important change as a new version.

Each version can be viewed, compared, or restored whenever needed.

---

# Types of Version Control

### Local Version Control

Stores history only on a single computer.

Example:

```
My Computer
```

---

### Centralized Version Control

A central server stores the project history.

Example:

```
Developer
      │
      ▼
Central Server
      ▲
      │
Developer
```

---

### Distributed Version Control

Every developer has a complete copy of the repository.

Git uses this model.

```
Developer A

Complete Repository

Developer B

Complete Repository
```

---

# Why Git is Popular

Git is:

- Fast
- Lightweight
- Reliable
- Distributed
- Open Source
- Widely used in the software industry

---

# Real-World Example

Suppose you are developing a portfolio website.

Day 1

```
Landing Page
```

Day 2

```
Add Navigation Bar
```

Day 3

```
Add About Section
```

Day 4

```
Bug Introduced
```

Without Git, recovering the previous working version is difficult.

With Git, you can simply restore an earlier version.

---

# Benefits of Version Control

- Tracks project history
- Restores previous versions
- Makes collaboration easier
- Identifies who changed what
- Helps resolve mistakes
- Supports experimentation safely

---

# Git Workflow (Basic)

```
Working Directory
        │
        ▼
Staging Area
        │
        ▼
Commit History
```

This workflow forms the foundation of Git.

---

# Key Terms

### Repository

A project managed by Git.

---

### Commit

A snapshot of the project at a specific point in time.

---

### History

A complete timeline of all commits.

---

### Branch

An independent line of development.

---

### HEAD

A reference to the latest commit on the current branch.

---

# Summary

Version control is one of the most important tools in modern software development.

Git allows developers to safely track project history, collaborate with teams, compare different versions, and recover from mistakes.

Learning version control before learning advanced Git commands provides a strong foundation for future development projects.

---

# Key Takeaways

- Version control records project history.
- Git is a distributed version control system.
- Every commit represents a snapshot of the project.
- Git makes collaboration simple.
- Previous versions can always be restored.

---

# What I Learned

After studying this chapter, I learned:

- Why version control is important.
- How Git stores project history.
- The difference between traditional file backups and Git.
- The advantages of using Git in software development.
- The basic Git workflow.