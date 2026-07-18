# Intermediate Git Cheat Sheet

A quick reference for the Git commands covered in the **Intermediate Git** course.

---

# Branch Management

## Create a Branch

```bash
git branch <branch-name>
```

## Switch to a Branch

```bash
git switch <branch-name>
```

## Create and Switch

```bash
git switch -c <branch-name>
```

## List Branches

```bash
git branch
```

## Rename Current Branch

```bash
git branch -m <new-name>
```

## Rename Another Branch

```bash
git branch -m <old-name> <new-name>
```

## Delete a Branch

```bash
git branch -d <branch-name>
```

---

# Comparing Branches

## Compare Two Branches

```bash
git diff main feature
```

---

# Merging

## Merge a Branch

```bash
git merge <branch-name>
```

Example:

```bash
git merge feature-login
```

---

# Merge Conflicts

## Open Conflicted File

```bash
nano README.md
```

## Stage Resolved File

```bash
git add README.md
```

## Complete Merge

```bash
git commit
```

---

# Remote Repositories

## Clone a Repository

```bash
git clone <repository-url>
```

Example:

```bash
git clone https://github.com/user/project.git
```

---

## List Remotes

```bash
git remote
```

---

## Show Remote Details

```bash
git remote -v
```

---

## Add a Remote

```bash
git remote add <name> <url>
```

Example:

```bash
git remote add origin https://github.com/user/project.git
```

---

# Synchronization

## Fetch Updates

```bash
git fetch origin
```

## Fetch Specific Branch

```bash
git fetch origin main
```

---

## Pull Updates

```bash
git pull origin main
```

---

## Pull Without Editing Merge Message

```bash
git pull --no-edit origin main
```

---

## Push Changes

```bash
git push origin main
```

---

## Push a New Branch

```bash
git push origin feature-login
```

---

# Typical Team Workflow

```bash
git pull origin main

# Make changes

git add .

git commit -m "Your commit message"

git push origin main
```

---

# Fetch vs Pull

| Command | Downloads Changes | Merges Changes |
|----------|-------------------|----------------|
| `git fetch` | ✅ | ❌ |
| `git pull` | ✅ | ✅ |

---

# Pull vs Push

| Command | Direction |
|----------|-----------|
| `git pull` | Remote → Local |
| `git push` | Local → Remote |

---

# Best Practices

- Pull before starting new work.
- Commit small and meaningful changes.
- Push regularly.
- Use feature branches.
- Resolve merge conflicts carefully.
- Keep repositories synchronized.
- Review changes before merging.

---

# Common Workflow

```text
Clone Repository

↓

Create Branch

↓

Develop Feature

↓

Commit Changes

↓

Merge Branch

↓

Pull Latest Updates

↓

Push Changes

↓

Collaborate
```