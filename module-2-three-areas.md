# Module 2: Understanding Git's Three Areas

## Learning Objectives

By the end of this module, you will be able to:
- Explain the three areas of a Git repository
- Understand why the staging area exists
- Move files between areas intentionally
- Use selective staging to create clean commits

---

## Why This Matters

This is the **most important conceptual model** in Git. Every Git command moves data between these three areas. Once you understand this, all Git operations will make sense. Without this mental model, Git feels like magic (or frustration).

---

## Key Concepts

### The Three Areas

Every Git repository has three distinct areas:

```
┌────────────────────────────────────────────────────────────────────────────┐
│                                                                            │
│   WORKING DIRECTORY         STAGING AREA            REPOSITORY             │
│   (Working Tree)            (Index)                 (.git directory)       │
│                                                                            │
│   ┌──────────────┐         ┌──────────────┐        ┌──────────────┐        │
│   │              │         │              │        │              │        │
│   │  Your files  │ ─────── │   Prepared   │ ────── │  Committed   │        │
│   │  as you see  │ git add │   changes    │ commit │  snapshots   │        │
│   │  them        │         │   (next      │        │  (history)   │        │
│   │              │         │    commit)   │        │              │        │
│   └──────────────┘         └──────────────┘        └──────────────┘        │
│                                                                            │
│   • Untracked files        • "Changes to be       • Permanent record       │
│   • Modified files           committed"           • Each commit has ID     │
│   • What you edit          • Your "shopping       • Safe and recoverable   │
│                              cart"                                         │
│                                                                            │
└────────────────────────────────────────────────────────────────────────────┘
```

### Area 1: Working Directory

The **Working Directory** is simply your project folder—the files you see in your file explorer or editor.

- Contains all your actual files
- Where you create, edit, and delete files
- Changes here are not saved to Git until you stage and commit them
- Files can be:
  - **Untracked**: New files Git doesn't know about
  - **Tracked & Unmodified**: Files Git knows about, unchanged since last commit
  - **Tracked & Modified**: Files Git knows about, with changes since last commit

### Area 2: Staging Area (Index)

The **Staging Area** is a holding zone for changes you're preparing to commit.

- Also called the "Index"
- Contains changes marked for the next commit
- Think of it as a "loading dock" or "shopping cart"
- You choose what goes here—not everything has to be staged
- Allows you to build commits piece by piece

### Area 3: Repository (.git directory)

The **Repository** is the committed history stored in the hidden `.git` folder.

- Contains all commits (snapshots) ever made
- Each commit is permanent and identified by a unique SHA hash
- This is your "save file"—you can always go back here
- Never edit the `.git` folder directly

---

## Why Have a Staging Area?

Many people ask: "Why not commit directly from the working directory?"

The staging area gives you **control and precision**:

| Benefit | Example |
|---------|---------|
| **Selective commits** | Changed 5 files but only want to commit 2? Stage just those 2. |
| **Logical grouping** | Keep bug fixes and new features in separate commits. |
| **Review before saving** | Double-check what you're about to commit with `git diff --staged`. |
| **Partial file staging** | Stage only some changes within a file (with `git add -p`). |

Think of it like packing for a trip: you don't throw everything from your closet into your suitcase. You choose what to pack (staging), then close the suitcase (commit).

---

## Commands for Moving Between Areas

```
                        git add                   git commit
Working Directory ──────────────► Staging Area ──────────────► Repository

                       git restore              git restore --staged
Working Directory ◄──────────────  Staging Area ◄──────────────
                     (discard)                    (unstage)
```

| From | To | Command |
|------|-----|---------|
| Working Directory | Staging Area | `git add <file>` |
| Staging Area | Repository | `git commit -m "message"` |
| Staging Area | Working Directory | `git restore --staged <file>` |
| Last Commit | Working Directory | `git restore <file>` |

---

## Exercise 2.1: Observe the Three Areas

Make sure you're in your repository from Module 1, or create a new one:

```bash
# If you need a fresh start:
mkdir three-areas-demo
cd three-areas-demo
git init
echo "# Demo Project" > README.md
git add README.md
git commit -m "Initial commit"
```

Now let's create files and watch them move through the areas:

```bash
# Create two new files (Working Directory - Untracked)
echo "Content for file A" > file-a.txt
echo "Content for file B" > file-b.txt

# Check status - both files are untracked
git status
```

**Expected:** Both files shown as "Untracked files"

```bash
# Stage only file-a.txt (moves to Staging Area)
git add file-a.txt

# Check status again
git status
```

**Expected:**
- `file-a.txt` is under "Changes to be committed" (staged)
- `file-b.txt` is still under "Untracked files" (working directory only)

```bash
# Commit only what's staged
git commit -m "Add file A"

# Check status
git status
```

**Expected:** `file-a.txt` is now in the repository (not shown). `file-b.txt` is still untracked.

```bash
# Stage and commit file B
git add file-b.txt
git commit -m "Add file B"

# View history - two separate commits!
git log --oneline
```

---

## Exercise 2.2: Modify and Stage Selectively

```bash
# Modify both files
echo "New line in file A" >> file-a.txt
echo "New line in file B" >> file-b.txt

# Check status - both are modified
git status
```

**Expected:** Both files shown as "modified"

```bash
# Stage only file-a.txt
git add file-a.txt

# Check status
git status
```

**Expected:**
- `file-a.txt` is under "Changes to be committed"
- `file-b.txt` is under "Changes not staged for commit"

```bash
# See what's staged vs. not staged
echo "=== Staged changes (will be committed) ==="
git diff --staged

echo ""
echo "=== Unstaged changes (won't be committed) ==="
git diff
```

```bash
# Commit only file-a.txt
git commit -m "Update file A with new line"

# file-b.txt is still modified but not committed
git status

# Now stage and commit file-b.txt
git add file-b.txt
git commit -m "Update file B with new line"
```

---

## Exercise 2.3: Unstaging Files

Sometimes you stage something by mistake. Here's how to unstage:

```bash
# Modify a file
echo "Accidental change" >> file-a.txt

# Stage it (oops, didn't mean to!)
git add file-a.txt

# Check status - it's staged
git status

# Unstage it (move back to working directory)
git restore --staged file-a.txt

# Check status - now it's modified but not staged
git status

# If you want to discard the change entirely:
git restore file-a.txt

# Check status - clean working directory
git status
```

---

## Exercise 2.4: Understanding File States

Let's see all possible file states:

```bash
# Create a new file (Untracked)
echo "I'm new here" > new-file.txt
git status

# Stage it (Staged/New)
git add new-file.txt
git status

# Commit it (now it's Tracked)
git commit -m "Add new file"
git status

# Modify it (Modified/Unstaged)
echo "Adding more content" >> new-file.txt
git status

# Stage the modification (Modified/Staged)
git add new-file.txt
git status

# Make another change WITHOUT staging (both staged and unstaged changes!)
echo "Even more content" >> new-file.txt
git status
```

**Final status should show:**
- `new-file.txt` under BOTH "Changes to be committed" AND "Changes not staged"
- This means part of your changes are staged, and newer changes are not

```bash
# See the difference:
echo "=== What's staged ==="
git diff --staged

echo ""
echo "=== What's NOT staged ==="
git diff
```

---

## Visual Summary

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        FILE LIFECYCLE IN GIT                            │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│   Untracked ─────► Staged ─────► Committed                              │
│       │           git add      git commit                               │
│       │              │              │                                   │
│       │              │              ▼                                   │
│       │              │         Unmodified ◄──┐                          │
│       │              │              │        │                          │
│       │              │         (edit file)   │                          │
│       │              │              │        │                          │
│       │              │              ▼        │                          │
│       │              └──────── Modified      │                          │
│       │                            │         │                          │
│       │                        git add       │                          │
│       │                            │         │                          │
│       │                            ▼         │                          │
│       │                        Staged ───────┘                          │
│       │                       git commit                                │
│       │                                                                 │
│       │                                                                 │
│       └── Remove file from tracking: git rm --cached <file>             │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference

```bash
# See current state of all three areas
git status

# See unstaged changes (Working Dir vs Staging)
git diff

# See staged changes (Staging vs Last Commit)
git diff --staged

# Move file: Working Directory → Staging Area
git add <file>

# Move all files: Working Directory → Staging Area
git add .

# Move file: Staging Area → Working Directory (unstage)
git restore --staged <file>

# Discard changes in Working Directory
git restore <file>

# Move staged changes → Repository
git commit -m "message"
```

---

## Key Takeaways

1. **Three areas**: Working Directory → Staging Area → Repository
2. **Staging is intentional**: You control exactly what goes into each commit
3. **`git status` shows all three areas**: Learn to read its output
4. **`git diff` vs `git diff --staged`**: Different comparisons for different purposes
5. **You can have both staged and unstaged changes** in the same file

---

## Common Mistakes to Avoid

| Mistake | What Happens | Solution |
|---------|--------------|----------|
| `git add .` without checking status | Staging unintended files | Run `git status` first, or use `git add <specific-file>` |
| Forgetting `--staged` with diff | See wrong changes | Remember: `git diff` = unstaged, `git diff --staged` = staged |
| Editing a file after staging | Creates split state | Either stage again or commit what you have |

---

**Previous Module:** [Module 1 - Quickstart](./module-1-quickstart.md)  
**Next Module:** [Module 3 - Core Commands Deep Dive](./module-3-core-commands.md)
