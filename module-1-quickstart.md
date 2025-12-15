# Module 1: Quickstart - Your First Repository

## Learning Objectives

By the end of this module, you will be able to:
- Initialize a new Git repository
- Understand what `git init` creates
- Create and track files
- Make your first commit
- View basic commit history

---

## Why This Matters

Every Git workflow starts with these fundamentals. Whether you're working on a solo project or joining a team, understanding how to initialize a repository and make commits is essential. This module gets you hands-on immediately so you can build confidence with real commands.

---

## Key Concepts

### What is a Repository?

A **repository** (or "repo") is a project folder that Git is tracking. It contains:
- Your project files (code, documents, etc.)
- A hidden `.git` folder where Git stores all history and metadata

### The Basic Workflow

Every change in Git follows this pattern:

```
Edit Files → Stage Changes → Commit
              (git add)     (git commit)
```

1. **Edit** — Make changes to your files normally
2. **Stage** — Tell Git which changes you want to save (`git add`)
3. **Commit** — Save a snapshot of those changes (`git commit`)

### What is a Commit?

A **commit** is a snapshot of your project at a specific point in time. Each commit:
- Has a unique ID (a long hash like `a1b2c3d4...`)
- Contains a message describing the change
- Records who made it and when
- Points to the previous commit (creating a history chain)

---

## Commands Introduced

| Command | Purpose |
|---------|---------|
| `git init` | Create a new Git repository |
| `git status` | Show the current state of your repo |
| `git add <file>` | Stage a file for commit |
| `git commit -m "message"` | Save staged changes with a message |
| `git log` | View commit history |

---

## Exercise 1.1: Create Your First Repository

Open your terminal and run each command, observing the output:

```bash
# Step 1: Create a new project directory
mkdir my-first-repo
cd my-first-repo

# Step 2: Initialize a Git repository
git init
```

**What happened?** Git created a hidden `.git` folder. You can verify:

```bash
ls -la
```

You should see a `.git` directory. This is where Git stores everything—never edit this folder directly.

---

## Exercise 1.2: Check Repository Status

```bash
# Check the status of your repository
git status
```

**Expected output:**
```
On branch main
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

This tells you:
- You're on the `main` branch
- No commits exist yet
- There's nothing to commit (no files yet)

---

## Exercise 1.3: Create and Track a File

```bash
# Create a README file
echo "# My First Project" > README.md
echo "This is my first Git repository!" >> README.md

# View the file contents
cat README.md

# Check status again
git status
```

**Expected output:**
```
On branch main
No commits yet
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

**Key observation:** Git sees the new file but isn't tracking it yet. It's "untracked."

---

## Exercise 1.4: Stage the File

```bash
# Stage the file (add it to the staging area)
git add README.md

# Check status
git status
```

**Expected output:**
```
On branch main
No commits yet
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md
```

**Key observation:** The file moved from "Untracked" to "Changes to be committed." It's now staged and ready to be saved.

---

## Exercise 1.5: Make Your First Commit

```bash
# Commit the staged changes with a descriptive message
git commit -m "Initial commit: Add README"
```

**Expected output:**
```
[main (root-commit) abc1234] Initial commit: Add README
 1 file changed, 2 insertions(+)
 create mode 100644 README.md
```

**Key observation:** Git created a commit with:
- A unique ID (the `abc1234` part)
- Your message
- A record of what changed (1 file, 2 lines added)

---

## Exercise 1.6: View Your History

```bash
# View the commit log
git log
```

**Expected output:**
```
commit abc1234567890... (HEAD -> main)
Author: Your Name <your@email.com>
Date:   Mon Jan 1 12:00:00 2025 -0500

    Initial commit: Add README
```

For a more compact view:

```bash
git log --oneline
```

**Output:**
```
abc1234 Initial commit: Add README
```

---

## Exercise 1.7: Make Another Change

Let's practice the full workflow again:

```bash
# Edit the file
echo "" >> README.md
echo "## About" >> README.md
echo "This project demonstrates basic Git usage." >> README.md

# Check what changed
git status

# Stage the changes
git add README.md

# Commit with a message
git commit -m "Add About section to README"

# View updated history
git log --oneline
```

You should now see two commits in your history.

---

## Understanding What Just Happened

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│   README.md created     README.md staged     Commit created     │
│         │                     │                    │            │
│         ▼                     ▼                    ▼            │
│   ┌──────────┐          ┌──────────┐         ┌──────────┐      │
│   │ Working  │  ──────► │ Staging  │ ──────► │   Repo   │      │
│   │Directory │  git add │   Area   │  commit │ History  │      │
│   └──────────┘          └──────────┘         └──────────┘      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference

```bash
# Initialize a new repository
git init

# Check current status
git status

# Stage a file
git add <filename>

# Stage all files
git add .

# Commit staged changes
git commit -m "Your message here"

# View commit history
git log
git log --oneline
```

---

## Key Takeaways

1. **`git init`** creates a new repository by adding a `.git` folder to your project
2. **`git status`** is your window into what Git sees—use it frequently
3. **Files must be staged before committing**—this gives you control over what goes into each commit
4. **Commits are snapshots**—each one captures the state of your staged files at that moment
5. **Good commit messages matter**—they help you (and others) understand the history

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Forgetting to stage files | Always run `git status` before committing |
| Vague commit messages like "fix" | Be specific: "Fix null check in user validation" |
| Committing too many unrelated changes | Stage and commit related changes together |

---

**Previous Module:** [Module 0 - Getting Help](./module-0-getting-help.md)  
**Next Module:** [Module 2 - Understanding Git's Three Areas](./module-2-three-areas.md)
