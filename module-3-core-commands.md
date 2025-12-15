# Module 3: Core Commands Deep Dive

## Learning Objectives

By the end of this module, you will be able to:
- Use `git status` effectively to understand repository state
- Navigate commit history with `git log` and its options
- Compare changes with `git diff` at different stages
- Write meaningful commit messages
- Use shortcuts to speed up your workflow

---

## Why This Matters

These are the commands you'll use every single day. Mastering them will make you faster, help you avoid mistakes, and give you confidence when working with Git. Think of these as your core vocabulary—the words you need before you can speak the language fluently.

---

## Key Concepts

### The Four Essential Commands

| Command | Question It Answers |
|---------|---------------------|
| `git status` | "What's the current state of my repo?" |
| `git log` | "What happened in the past?" |
| `git diff` | "What exactly changed?" |
| `git commit` | "Save this snapshot with a description" |

---

## Command 1: `git status`

Your **most-used command**. Run it constantly—before adding, before committing, whenever you're unsure.

### Basic Usage

```bash
git status
```

### What It Shows

```
On branch main                              ← Current branch
Your branch is up to date with 'origin/main'. 

Changes to be committed:                    ← Staged (green)
  (use "git restore --staged <file>..." to unstage)
        modified:   staged-file.txt

Changes not staged for commit:              ← Modified but not staged (red)
  (use "git add <file>..." to update what will be committed)
        modified:   modified-file.txt

Untracked files:                            ← New files Git doesn't know (red)
  (use "git add <file>..." to include in what will be committed)
        new-file.txt
```

### Short Status

For a compact view:

```bash
git status -s
```

Output uses two-character codes:

| Code | Meaning |
|------|---------|
| `??` | Untracked (new file) |
| `A ` | Added (staged new file) |
| `M ` | Modified and staged |
| ` M` | Modified but not staged |
| `MM` | Modified, staged, then modified again |
| `D ` | Deleted and staged |
| ` D` | Deleted but not staged |

---

## Command 2: `git log`

View the history of commits.

### Basic Usage

```bash
git log
```

Shows full details: commit hash, author, date, and message.

### Useful Variations

```bash
# Compact one-line format (most useful for quick viewing)
git log --oneline

# Show last N commits
git log --oneline -5

# Show what files changed in each commit
git log --stat

# Show the actual changes (diffs) in each commit
git log -p

# Show commits by a specific author
git log --author="Name"

# Show commits containing a search term in the message
git log --grep="bug fix"

# Show commits that changed a specific file
git log -- filename.txt

# Visual graph (very useful with branches)
git log --oneline --graph --all
```

### Reading Log Output

```
commit 8a7b9c3d4e5f6... (HEAD -> main)   ← Full commit hash, current position
Author: Your Name <email@example.com>    ← Who made the commit
Date:   Mon Jan 6 14:30:00 2025 -0500   ← When

    Add user authentication feature       ← Commit message
```

---

## Command 3: `git diff`

Shows exactly what changed, line by line.

### Three Types of Diff

```bash
# What changed in working directory (not yet staged)
git diff

# What's staged (will be in next commit)
git diff --staged
# or: git diff --cached (same thing)

# Difference between any two commits
git diff <commit1> <commit2>
```

### Reading Diff Output

```diff
diff --git a/file.txt b/file.txt
index 1234567..abcdefg 100644
--- a/file.txt                    ← Old version
+++ b/file.txt                    ← New version
@@ -1,4 +1,5 @@                   ← Location: line numbers
 Line 1 unchanged                  ← Context (no prefix)
-Line 2 old version               ← Removed (red, minus sign)
+Line 2 new version               ← Added (green, plus sign)
+Line 3 completely new            ← Added (green, plus sign)
 Line 4 unchanged                  ← Context
```

### Useful Diff Options

```bash
# Show only names of changed files
git diff --name-only

# Show names and status (added, modified, deleted)
git diff --name-status

# Show word-level changes instead of line-level
git diff --word-diff

# Diff a specific file
git diff filename.txt
git diff --staged filename.txt
```

---

## Command 4: `git commit`

Creates a permanent snapshot of your staged changes.

### Basic Usage

```bash
# Commit with inline message
git commit -m "Your descriptive message"

# Stage all tracked files AND commit in one step
git commit -am "Your message"
# Note: This doesn't add untracked (new) files!

# Open editor for longer message
git commit
```

### Writing Good Commit Messages

**The Golden Rule:** Your message should complete the sentence "If applied, this commit will..."

**Good examples:**
- `Add user login validation`
- `Fix memory leak in image processor`
- `Update README with installation steps`
- `Remove deprecated API endpoints`

**Bad examples:**
- `fix` (fix what?)
- `WIP` (work in progress tells us nothing)
- `updates` (what updates?)
- `asdfasdf` (not helpful)

### Commit Message Format

For longer messages:

```
Short summary (50 chars or less)

More detailed explanation if needed. Wrap at 72 characters.
Explain the "why" not just the "what" - the diff shows what changed.

- Bullet points are okay
- Keep them concise

Refs: #123 (reference issue numbers if applicable)
```

### Amending Commits

Made a typo or forgot to add a file?

```bash
# Add forgotten file to last commit
git add forgotten-file.txt
git commit --amend

# Just fix the commit message
git commit --amend -m "New message"
```

**Warning:** Only amend commits that haven't been pushed to a shared repository!

---

## Exercise 3.1: Practice Status and Diff

```bash
# Start fresh or use existing repo
mkdir core-commands-demo
cd core-commands-demo
git init

# Create initial file
echo "Line 1: Hello" > demo.txt
git add demo.txt
git commit -m "Initial commit"

# Check clean status
git status

# Modify the file
echo "Line 2: World" >> demo.txt

# Check status - file is modified
git status
git status -s

# See what changed (unstaged)
git diff

# Stage it
git add demo.txt

# Now diff shows nothing (no unstaged changes)
git diff

# But --staged shows what will be committed
git diff --staged

# Make another change WITHOUT staging
echo "Line 3: More content" >> demo.txt

# Now we have both staged AND unstaged changes
git status

# See the difference
echo "=== UNSTAGED (new changes) ==="
git diff
echo ""
echo "=== STAGED (ready to commit) ==="
git diff --staged

# Commit only staged changes
git commit -m "Add Line 2"

# Line 3 is still there, still modified
git status
cat demo.txt

# Now commit Line 3
git commit -am "Add Line 3"
```

---

## Exercise 3.2: Explore History

```bash
# Create more commits to have history to explore
echo "Line 4: Additional content" >> demo.txt
git commit -am "Add Line 4"

echo "Line 5: Even more" >> demo.txt
git commit -am "Add Line 5"

echo "Line 6: Final line" >> demo.txt
git commit -am "Add Line 6"

# View full history
git log

# View compact history
git log --oneline

# View last 3 commits
git log --oneline -3

# View with statistics
git log --stat -3

# View with actual changes
git log -p -1

# View specific commit details
git show HEAD
git show HEAD~1    # One commit before HEAD
git show HEAD~2    # Two commits before HEAD
```

---

## Exercise 3.3: Compare Commits

```bash
# Compare current state to 3 commits ago
git diff HEAD~3

# Compare two specific commits
git log --oneline  # Note the commit hashes
git diff <older-hash> <newer-hash>

# See what changed in a specific file over time
git log --oneline -- demo.txt
git diff HEAD~3 -- demo.txt
```

---

## Shortcuts and Time-Savers

### Useful Aliases

You can create shortcuts for common commands:

```bash
# Set up aliases
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.lg "log --oneline --graph --all"

# Now use them
git st        # instead of git status
git lg        # pretty log graph
```

### Common Shortcuts

```bash
# Stage and commit tracked files in one command
git commit -am "message"

# Stage all files in current directory
git add .

# Stage all files everywhere (including deletions)
git add -A

# Show brief status
git status -s

# Show last commit
git show
```

---

## Quick Reference

### git status
```bash
git status              # Full status
git status -s           # Short status
```

### git log
```bash
git log                 # Full log
git log --oneline       # Compact log
git log --oneline -5    # Last 5 commits
git log --stat          # With file stats
git log -p              # With diffs
git log --graph --all   # Visual branch graph
git log -- <file>       # History of specific file
```

### git diff
```bash
git diff                # Unstaged changes
git diff --staged       # Staged changes
git diff <commit>       # Compare to specific commit
git diff <c1> <c2>      # Compare two commits
git diff --name-only    # Just filenames
```

### git commit
```bash
git commit -m "msg"     # Commit with message
git commit -am "msg"    # Stage tracked + commit
git commit --amend      # Modify last commit
```

---

## Key Takeaways

1. **`git status` is your safety net** — Run it constantly to understand your state
2. **`git log --oneline` is quick and useful** — Full log is rarely needed
3. **Know both `git diff` and `git diff --staged`** — They show different things
4. **Write meaningful commit messages** — Your future self will thank you
5. **`git commit -am` is a shortcut** — But only for tracked files

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Using `git commit -am` for new files | New files need explicit `git add` first |
| Forgetting `--staged` with diff | Remember: `diff` = unstaged, `diff --staged` = staged |
| Amending pushed commits | Only amend local, unpushed commits |
| Not reading status output | Status tells you exactly what to do next |

---

**Previous Module:** [Module 2 - Three Areas](./module-2-three-areas.md)  
**Next Module:** [Module 4 - Branching Basics](./module-4-branching-basics.md)
