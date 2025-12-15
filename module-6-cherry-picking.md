# Module 6: Cherry-Picking

## Learning Objectives

By the end of this module, you will be able to:
- Explain what cherry-picking is and when to use it
- Apply specific commits from one branch to another
- Understand the difference between cherry-pick and merge
- Recognize when cherry-picking is appropriate vs. problematic

---

## Why This Matters

Sometimes you need a specific commit from another branch without merging the entire branch. Maybe a bug fix was made on a feature branch that you need in production immediately, or a commit was accidentally made on the wrong branch. Cherry-picking lets you surgically select individual commits and apply them elsewhere.

---

## Key Concepts

### What is Cherry-Picking?

**Cherry-picking** copies a specific commit from one branch and applies it to another branch. It creates a new commit with the same changes but a different commit hash.

Think of it like this: if merging is "take everything from that branch," cherry-picking is "take just that one commit."

### Before Cherry-Pick

```
main      ●────●────●────●
                    A    B

feature   ●────●────●────●────●
                    C    D    E
```

You want commit D on main, but not C or E.

### After Cherry-Picking Commit D onto Main

```
main      ●────●────●────●────●
                    A    B    D'   (D' is a copy of D)

feature   ●────●────●────●────●
                    C    D    E
```

Notice:
- D' is a **new commit** (different hash than D)
- D' contains the **same changes** as D
- Commits C and E are not included
- The feature branch is unchanged

### How Cherry-Pick Works

1. Git looks at the commit you specified
2. Git calculates what changes that commit introduced
3. Git applies those same changes to your current branch
4. Git creates a new commit with those changes

---

## When to Use Cherry-Picking

### Good Use Cases

| Scenario | Example |
|----------|---------|
| **Hotfix needed on main** | A critical bug was fixed on develop; you need it in production now |
| **Commit on wrong branch** | You accidentally committed to main instead of your feature branch |
| **Backporting fixes** | Applying a fix to an older release branch |
| **Selective feature inclusion** | Taking one specific change without the whole feature |

### When NOT to Cherry-Pick

| Scenario | Better Alternative |
|----------|-------------------|
| You want all commits from a branch | Use `git merge` |
| Regular workflow between branches | Use `git merge` or `git rebase` |
| The commit depends on previous commits | Cherry-pick all dependencies or merge |
| You'll merge the branch later anyway | Just wait and merge |

### The Duplicate Commit Warning

Cherry-picking creates duplicate changes in your history. If you later merge the original branch, Git usually handles this gracefully, but it can cause confusion:

```
main      ●────●────●────D'────●────M
                              \    /
feature   ●────●────C────D────E───┘
```

When you merge, both D and D' exist in history (with the same changes). Git is usually smart enough to handle this, but it can make history harder to read.

---

## Cherry-Pick Commands

| Command | Purpose |
|---------|---------|
| `git cherry-pick <commit>` | Apply a single commit |
| `git cherry-pick <c1> <c2> <c3>` | Apply multiple commits |
| `git cherry-pick <c1>..<c2>` | Apply a range of commits (excludes c1) |
| `git cherry-pick <c1>^..<c2>` | Apply a range including c1 |
| `git cherry-pick --no-commit <commit>` | Apply changes without committing |
| `git cherry-pick --abort` | Cancel an in-progress cherry-pick |
| `git cherry-pick --continue` | Continue after resolving conflicts |

---

## Exercise 6.1: Basic Cherry-Pick

Let's practice cherry-picking a commit from one branch to another.

### Setup

```bash
# Create a new repo for this exercise
mkdir cherry-pick-demo
cd cherry-pick-demo
git init

# Create initial commit on main
echo "# Cherry Pick Demo" > README.md
git add README.md
git commit -m "Initial commit"

# Create a feature branch with multiple commits
git switch -c feature/new-stuff

echo "Feature work part 1" > feature.txt
git add feature.txt
git commit -m "Add feature part 1"

echo "Important bug fix!" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical bug"

echo "Feature work part 2" >> feature.txt
git commit -am "Add feature part 2"

# View the feature branch history
echo "=== Feature branch commits ==="
git log --oneline
```

You should see something like:
```
abc1234 Add feature part 2
def5678 Fix critical bug      <-- We want THIS commit on main
ghi9012 Add feature part 1
jkl3456 Initial commit
```

### Cherry-Pick the Bug Fix

```bash
# Note the commit hash for "Fix critical bug"
# (Use the actual hash from your git log output)
BUG_FIX_HASH=$(git log --oneline | grep "Fix critical bug" | cut -d' ' -f1)
echo "Bug fix commit hash: $BUG_FIX_HASH"

# Switch to main
git switch main

# Verify bugfix.txt doesn't exist on main
ls

# Cherry-pick the bug fix commit
git cherry-pick $BUG_FIX_HASH

# Verify the bug fix is now on main
ls
cat bugfix.txt

# View main's history
echo "=== Main branch after cherry-pick ==="
git log --oneline
```

### Verify the Results

```bash
# Main has the bug fix but NOT the feature work
ls
# Should show: README.md, bugfix.txt (no feature.txt)

# Compare the branches
echo "=== Main branch ==="
git log main --oneline

echo ""
echo "=== Feature branch ==="
git log feature/new-stuff --oneline

# Notice: different commit hashes for the bug fix
```

---

## Exercise 6.2: Cherry-Pick Multiple Commits

```bash
# Still in the same repo, let's add more commits to feature
git switch feature/new-stuff

echo "Documentation update" > docs.txt
git add docs.txt
git commit -m "Add documentation"

echo "Another important fix" > fix2.txt
git add fix2.txt
git commit -m "Fix second issue"

echo "More feature work" >> feature.txt
git commit -am "Expand feature"

# View all commits
git log --oneline
```

Now cherry-pick multiple specific commits:

```bash
# Get the hashes for the commits we want
DOC_HASH=$(git log --oneline | grep "Add documentation" | cut -d' ' -f1)
FIX2_HASH=$(git log --oneline | grep "Fix second issue" | cut -d' ' -f1)

# Switch to main
git switch main

# Cherry-pick both commits
git cherry-pick $DOC_HASH $FIX2_HASH

# View the result
echo "=== Main branch after multiple cherry-picks ==="
git log --oneline

# Verify files
ls
```

---

## Exercise 6.3: Cherry-Pick Without Committing

Sometimes you want to apply changes but modify them before committing:

```bash
# Create another commit on feature
git switch feature/new-stuff

echo "Config setting A=1" > config.txt
echo "Config setting B=2" >> config.txt
git add config.txt
git commit -m "Add configuration"

CONFIG_HASH=$(git log --oneline -1 | cut -d' ' -f1)

# Switch to main
git switch main

# Cherry-pick WITHOUT auto-committing
git cherry-pick --no-commit $CONFIG_HASH

# The changes are staged but not committed
git status

# You can modify before committing
echo "Config setting C=3" >> config.txt

# Now commit with your own message
git add config.txt
git commit -m "Add configuration (customized for main)"

# View result
git log --oneline -3
cat config.txt
```

---

## Handling Cherry-Pick Conflicts

If the cherry-picked commit conflicts with your current branch, Git will pause and ask you to resolve:

```bash
# Git will show something like:
# CONFLICT (content): Merge conflict in filename.txt
# error: could not apply abc1234... Commit message
# hint: After resolving the conflicts, mark them with "git add"
# hint: and make a commit with "git cherry-pick --continue"
```

### Resolution Steps

```bash
# 1. Open the conflicting file and resolve conflicts
#    (Edit the file, remove conflict markers)

# 2. Mark as resolved
git add <conflicting-file>

# 3. Continue the cherry-pick
git cherry-pick --continue

# OR abort if you change your mind
git cherry-pick --abort
```

---

## Cherry-Pick vs. Merge vs. Rebase

| Aspect | Cherry-Pick | Merge | Rebase |
|--------|-------------|-------|--------|
| **What it does** | Copies specific commits | Combines branch histories | Replays commits on new base |
| **Creates new commits** | Yes (copies) | Yes (merge commit) | Yes (replayed commits) |
| **History** | Can create duplicates | Preserves all history | Linear history |
| **Use case** | Specific commits needed | Integrate complete branches | Clean up before merging |
| **Scope** | One or few commits | Entire branch | Entire branch |

---

## Quick Reference

```bash
# Cherry-pick single commit
git cherry-pick <commit-hash>

# Cherry-pick multiple commits
git cherry-pick <hash1> <hash2> <hash3>

# Cherry-pick without committing
git cherry-pick --no-commit <hash>

# Abort a cherry-pick in progress
git cherry-pick --abort

# Continue after resolving conflicts
git cherry-pick --continue

# Find commit hashes
git log --oneline
git log --oneline <branch-name>
```

---

## Key Takeaways

1. **Cherry-pick copies individual commits** — It doesn't move them; it creates new commits with the same changes
2. **New commits have different hashes** — Even though the changes are the same
3. **Use sparingly** — Cherry-picking can create confusing history if overused
4. **Good for hotfixes** — When you need a specific fix on multiple branches
5. **Not a substitute for merging** — If you need most commits, just merge the branch

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Cherry-picking commits that depend on others | Cherry-pick dependencies too, or just merge |
| Using cherry-pick as primary workflow | Use merge or rebase for regular integration |
| Forgetting you cherry-picked (then merging) | Track what you've cherry-picked |
| Cherry-picking merge commits | Avoid this; it's complex and error-prone |

---

**Previous Module:** [Module 5 - Branching Strategies](./module-5-branching-strategies.md)  
**Next Module:** [Module 7 - Remote Repositories](./module-7-remote-repositories.md)
