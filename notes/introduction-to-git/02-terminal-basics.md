# Terminal Basics

## Overview

The terminal (also called the command line or shell) is a text-based interface that allows users to interact with the operating system by typing commands. While many Git operations can be performed using graphical tools, the terminal provides full control over Git and is the standard environment used by developers.

Learning a few essential terminal commands is the first step toward becoming comfortable with Git.

---

# What is a Terminal?

A terminal is a program that accepts text commands and executes them on your operating system.

Instead of clicking buttons, you communicate with your computer by typing commands.

Examples of terminal applications include:

- Git Bash (Windows)
- Terminal (macOS)
- Linux Terminal

---

# Why Use the Terminal with Git?

Git was originally designed to be used from the command line.

Using the terminal allows you to:

- Access all Git features
- Execute commands quickly
- Better understand Git workflows
- Work consistently across different operating systems
- Troubleshoot problems more effectively

---

# Understanding the Current Directory

Every command runs inside a directory (folder).

To display your current working directory:

```bash
pwd
```

Example:

```text
/d/git/git-learning-notes
```

---

# Listing Files and Folders

Use the following command to list files and directories.

```bash
ls
```

Example:

```text
README.md
notes/
assets/
```

For a detailed list:

```bash
ls -l
```

To display hidden files:

```bash
ls -a
```

Example:

```text
.
..
.git
README.md
notes
```

The `.git` directory is hidden because its name begins with a dot.

---

# Changing Directories

Move into another folder using:

```bash
cd folder-name
```

Example:

```bash
cd notes
```

Move back one directory:

```bash
cd ..
```

Go directly to your home directory:

```bash
cd ~
```

---

# Creating a Directory

Create a new folder.

```bash
mkdir project
```

Example:

```bash
mkdir git-practice
```

---

# Creating a File

Create an empty file.

```bash
touch README.md
```

Example:

```bash
touch notes.md
```

---

# Removing Files

Delete a file.

```bash
rm file.txt
```

Delete a directory and its contents.

```bash
rm -r folder-name
```

Use these commands carefully because deleted files cannot be easily recovered.

---

# Clearing the Terminal

Clear previous output.

```bash
clear
```

On Windows Command Prompt:

```text
cls
```

---

# Checking Git Installation

Verify that Git is installed correctly.

```bash
git --version
```

Example:

```text
git version 2.xx.x
```

If a version number appears, Git has been installed successfully.

---

# Viewing Help

Display Git help.

```bash
git help
```

Help for a specific command:

```bash
git help commit
```

or

```bash
git commit --help
```

---

# Real-World Example

Suppose you want to create a new Git project.

```bash
mkdir git-learning-notes

cd git-learning-notes

git init
```

This sequence:

1. Creates a project folder.
2. Moves into the folder.
3. Initializes a Git repository.

---

# Common Beginner Mistakes

- Running Git commands in the wrong directory.
- Forgetting to use `cd` before initializing a repository.
- Accidentally deleting files using `rm`.
- Confusing the terminal with Git itself.

---

# Summary

The terminal is an essential tool for using Git effectively. Understanding basic navigation and file management commands makes working with Git much easier. Even when using graphical Git clients, knowing terminal basics helps troubleshoot problems and improves overall productivity.

---

# Key Takeaways

- The terminal allows you to interact with your operating system using commands.
- Git is primarily designed to be used from the command line.
- Learn basic navigation commands before learning advanced Git commands.
- Always verify your current directory before executing Git commands.
- Practice terminal commands regularly to become more comfortable.

---

# Practice Commands

```bash
pwd

ls

ls -a

cd notes

cd ..

mkdir test-folder

touch README.md

git --version

git help
```

---

# Exercises

- [ ] Display your current working directory.
- [ ] Create a new folder named `practice`.
- [ ] Move into the folder.
- [ ] Create a `README.md` file.
- [ ] Check your installed Git version.

---

# What I Learned

After studying this chapter, I learned:

- What a terminal is and why developers use it.
- How to navigate between directories.
- How to create and remove files and folders.
- How to check whether Git is installed.
- The basic terminal commands required for working with Git.