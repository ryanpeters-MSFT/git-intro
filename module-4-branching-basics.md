# Module 4: Branching Basics

## Learning Objectives

By the end of this module, you will be able to:
- Explain what branches are and why they're useful
- Create, switch between, and delete branches
- Merge branches together
- Understand what HEAD means
- Read a branch graph

---

## Why This Matters

Branches are Git's killer feature. They allow you to:
- **Work on features** without affecting the main code
- **Experiment safely** — if it doesn't work, just delete the branch
- **Collaborate** — each person works on their own branch
- **Context switch** — pause one task, work on another, come back

Branching in Git is fast and cheap (just a 41-byte file), so you should use branches liberally.

---

## Key Concepts

### What is a Branch?

A **branch** is simply a pointer to a commit. That's it.

When you create a branch, Git creates a tiny file containing the hash of a commit. When you make new commits on that branch, the pointer moves forward.

```
                          main
                            │
                            ▼
        ┌───┐    ┌───┐    ┌───┐
  │ A │───>│ B │───>│ C │
        └───┘    └───┘    └───┘
```

In this diagram:
- Each box is a commit (A, B, C)
- `main` is a branch pointing to commit C
- Arrows show parent relationships (C's parent is B, B's parent is A)

### What is HEAD?

**HEAD** is a special pointer that tells Git "where you are right now."

Usually, HEAD points to a branch name (like `main`), which in turn points to a commit:

```
        HEAD
          │
          ▼
        main
          │
          ▼
        ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │
        └───┘    └───┘    └───┘
```

When you make a new commit, both the branch pointer and HEAD move forward together.

### Creating a Branch

When you create a new branch, Git creates a new pointer at your current commit:

```
        HEAD
          │
          ▼
        main
          │
          ▼
        ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │◀── feature (new branch)
        └───┘    └───┘    └───┘
```

Both `main` and `feature` point to the same commit. No files are copied—it's instant.

### Switching Branches

When you switch to `feature`, HEAD moves:

```
                        main
                          │
                          ▼
        ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │◀── feature
        └───┘    └───┘    └───┘       ▲
                                      │
                                    HEAD
```

### Making Commits on a Branch

When you commit on `feature`, only `feature` moves forward:

```
                        main
                          │
                          ▼
        ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │
        └───┘    └───┘    └───┘
                            │
                            │    ┌───┐    ┌───┐
                            └───>│ D │───>│ E │◀── feature
                                 └───┘    └───┘       ▲
                                                      │
                                                    HEAD
```

Now:
- `main` still points to C
- `feature` points to E (with D and E as new commits)
- HEAD points to `feature`

### Merging

When you merge `feature` into `main`, Git creates a merge commit:

```
                                      main
                                        │
                                        ▼
        ┌───┐    ┌───┐    ┌───┐      ┌───┐
        │ A │───>│ B │───>│ C │─────>│ F │◀── HEAD
        └───┘    └───┘    └───┘      └───┘
                            │          ▲
                            │    ┌───┐ │  ┌───┐
                            └───>│ D │─┴─>│ E │◀── feature
                                 └───┘    └───┘
```

Commit F is a **merge commit** — it has two parents (C and E).

---

## Essential Branch Commands

| Command | Purpose |
|---------|---------|
| `git branch` | List all branches |
| `git branch <name>` | Create a new branch |
| `git switch <name>` | Switch to a branch |
| `git switch -c <name>` | Create AND switch to new branch |
| `git merge <name>` | Merge a branch into current branch |
| `git branch -d <name>` | Delete a branch (if merged) |
| `git branch -D <name>` | Force delete a branch |

### Legacy Command Note

You may see `git checkout` used for switching branches in older tutorials. The modern equivalent is `git switch`, which is clearer and safer:

```bash
# Old way
git checkout feature

# New way (recommended)
git switch feature
```

---

## Exercise 4.1: Create and Switch Branches

```bash
# Create a new repo or use existing
mkdir branching-demo
cd branching-demo
git init

# Create initial commit
echo "# Branching Demo" > README.md
git add README.md
git commit -m "Initial commit"

# List branches (only main exists)
git branch

# Create a new branch called "feature"
git branch feature

# List branches again (* shows current branch)
git branch

# Switch to the feature branch
git switch feature

# Verify you're on feature
git branch
```

---

## Exercise 4.2: Work on a Branch

```bash
# Make sure you're on the feature branch
git switch feature

# Create a new file
echo "This is a new feature" > feature.txt

# Add and commit
git add feature.txt
git commit -m "Add feature file"

# Check the log - shows commits on this branch
git log --oneline

# Switch back to main
git switch main

# Notice: feature.txt doesn't exist on main!
ls

# Check log on main - doesn't include feature commit
git log --oneline
```

---

## Exercise 4.3: Visualize Branch History

```bash
# See all branches and their relationship
git log --oneline --graph --all
```

You should see something like:

```
* abc1234 (feature) Add feature file
* def5678 (HEAD -> main) Initial commit
```

---

## Exercise 4.4: Merge a Branch

```bash
# Make sure you're on the branch you want to merge INTO
git switch main

# Merge feature into main
git merge feature -m "Merge feature branch"

# Now feature.txt exists on main too
ls

# View the merged history
git log --oneline --graph --all

# The branches now point to the same commit
git branch -v
```

---

## Exercise 4.5: Delete a Merged Branch

```bash
# Delete the feature branch (it's been merged, so -d works)
git branch -d feature

# Verify it's gone
git branch
```

---

## Exercise 4.6: Full Branch Workflow

Let's practice a complete workflow:

```bash
# Start on main
git switch main

# Create and switch to a new branch in one command
git switch -c add-documentation

# Create documentation
echo "## Installation" > INSTALL.md
echo "Run the following commands:" >> INSTALL.md
echo "1. Clone the repository" >> INSTALL.md
echo "2. Run setup script" >> INSTALL.md

# Commit on the branch
git add INSTALL.md
git commit -m "Add installation documentation"

# Add more to the documentation
echo "" >> INSTALL.md
echo "## Configuration" >> INSTALL.md
echo "Edit config.json to customize settings" >> INSTALL.md
git commit -am "Add configuration section to docs"

# View branch history
git log --oneline

# Switch to main and verify docs aren't there
git switch main
ls

# Merge the documentation branch
git merge add-documentation -m "Merge documentation updates"

# Verify docs are now on main
ls
cat INSTALL.md

# Clean up
git branch -d add-documentation

# Final history
git log --oneline --graph --all
```

---

## Understanding Merge Types

### Fast-Forward Merge

When the target branch hasn't changed since you branched off, Git just moves the pointer forward:

**Before:**
```
                        main
                          │
                          ▼
        ┌───┐    ┌───┐    ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │───>│ D │───>│ E │◀── feature
        └───┘    └───┘    └───┘    └───┘    └───┘       ▲
                                                        │
                                                      HEAD
```

**After `git merge feature` (fast-forward):**
```
                                                  main
                                                    │
                                                    ▼
        ┌───┐    ┌───┐    ┌───┐    ┌───┐    ┌───┐
        │ A │───>│ B │───>│ C │───>│ D │───>│ E │◀── feature
        └───┘    └───┘    └───┘    └───┘    └───┘       ▲
                                                        │
                                                      HEAD
```

No new commit is created—main just moves to where feature is.

### Three-Way Merge

When both branches have new commits, Git creates a merge commit:

**Before:**
```
                          main
                            │
                            ▼
                          ┌───┐
             ┌───────────>│ F │
             │            └───┘
        ┌───┐    ┌───┐    
        │ A │───>│ B │───>┌───┐    ┌───┐
        └───┘    └───┘    │ C │───>│ D │◀── feature
                          └───┘    └───┘       ▲
                                               │
                                             HEAD
```

**After `git switch main` then `git merge feature`:**
```
                                    main
                                      │
                                      ▼
                          ┌───┐    ┌───┐
             ┌───────────>│ F │───>│ G │◀── HEAD (merge commit)
             │            └───┘    └───┘
             │                       ▲
        ┌───┐    ┌───┐               │
        │ A │───>│ B │───>┌───┐    ┌───┐
        └───┘    └───┘    │ C │───>│ D │◀── feature
                          └───┘    └───┘
```

Commit G has two parents: F and D.

---

## Quick Reference

```bash
# List branches
git branch              # Local branches
git branch -a           # All branches (including remote)
git branch -v           # Branches with last commit

# Create branch
git branch <name>       # Create only
git switch -c <name>    # Create and switch

# Switch branch
git switch <name>

# Merge branch (merge X into current branch)
git switch main         # First, go to target branch
git merge <name>        # Then merge

# Delete branch
git branch -d <name>    # Safe delete (must be merged)
git branch -D <name>    # Force delete

# Visualize
git log --oneline --graph --all
```

---

## Key Takeaways

1. **A branch is just a pointer** — Creating branches is instant and cheap
2. **HEAD points to your current location** — Usually a branch name
3. **Always know what branch you're on** — Use `git branch` or `git status`
4. **Merge INTO the target branch** — Switch to main, then merge feature
5. **Delete branches after merging** — Keeps things clean
6. **Use `git log --oneline --graph --all`** — Visualize your branch structure

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Merging from wrong branch | Always `git switch` to the target branch first |
| Forgetting which branch you're on | Run `git status` or `git branch` |
| Deleting unmerged branches | Git warns you; use `-D` only if you're sure |
| Working on main directly | Create a feature branch instead |

---

**Previous Module:** [Module 3 - Core Commands](./module-3-core-commands.md)  
**Next Module:** [Module 5 - Branching Strategies](./module-5-branching-strategies.md)
