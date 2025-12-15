# Module 8: Understanding Merge Conflicts

## Learning Objectives

By the end of this module, you will be able to:
- Understand what causes merge conflicts
- Recognize conflict markers in files
- Resolve conflicts manually
- Complete a merge after resolving conflicts
- Use strategies to minimize conflicts

---

## Why This Matters

Merge conflicts are inevitable when multiple people work on the same codebase—or even when you work on multiple branches yourself. They're not errors or failures; they're Git asking for human judgment when it can't automatically combine changes.

Understanding conflicts removes the fear. Once you know what's happening, they're straightforward to resolve.

---

## Key Concepts

### What is a Merge Conflict?

A **merge conflict** occurs when Git cannot automatically combine changes from two branches. This happens when:

- The same line(s) in a file were changed differently in both branches
- A file was modified in one branch and deleted in another
- The same file was added with different content in both branches

### When Git CAN Auto-Merge

Git is smart about combining changes. These scenarios merge cleanly:

```
Branch A changed:    Lines 1-10
Branch B changed:    Lines 50-60
Result:              Both changes applied automatically ✓
```

```
Branch A added:      new-file-a.txt
Branch B added:      new-file-b.txt
Result:              Both files included automatically ✓
```

### When Git CANNOT Auto-Merge

These scenarios cause conflicts:

```
Branch A changed:    Line 5 says "Hello World"
Branch B changed:    Line 5 says "Hello Universe"
Result:              CONFLICT - which one is correct?
```

Git doesn't know which change you want, so it asks you to decide.

---

## Anatomy of a Conflict

When a conflict occurs, Git modifies the affected file with **conflict markers**:

```
<<<<<<< HEAD
This is the content from your current branch (the one you're merging INTO)
=======
This is the content from the branch you're merging FROM
>>>>>>> feature-branch
```

### Breakdown of Conflict Markers

| Marker | Meaning |
|--------|---------|
| `<<<<<<< HEAD` | Start of conflict; content below is from your current branch |
| `=======` | Separator between the two versions |
| `>>>>>>> branch-name` | End of conflict; content above is from the incoming branch |

### Real-World Example

Imagine both branches modified a configuration file:

**Original (common ancestor):**
```
app_name = "MyApp"
version = "1.0"
debug = false
```

**Your branch (main) changed `version`:**
```
app_name = "MyApp"
version = "1.1"
debug = false
```

**Their branch (feature) changed `version` differently:**
```
app_name = "MyApp"
version = "2.0-beta"
debug = false
```

**After attempting merge, the file looks like:**
```
app_name = "MyApp"
<<<<<<< HEAD
version = "1.1"
=======
version = "2.0-beta"
>>>>>>> feature
debug = false
```

---

## How to Resolve a Conflict

### Step 1: Identify Conflicted Files

When a conflict occurs, Git tells you:

```bash
$ git merge feature
Auto-merging config.txt
CONFLICT (content): Merge conflict in config.txt
Automatic merge failed; fix conflicts and then commit the result.
```

Check status to see all conflicted files:

```bash
$ git status
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   config.txt
```

### Step 2: Open and Edit the File

Open the conflicted file in your editor. You'll see the conflict markers:

```
app_name = "MyApp"
<<<<<<< HEAD
version = "1.1"
=======
version = "2.0-beta"
>>>>>>> feature
debug = false
```

### Step 3: Decide What the Final Content Should Be

You have several options:

**Option A: Keep your version (HEAD)**
```
app_name = "MyApp"
version = "1.1"
debug = false
```

**Option B: Keep their version (feature)**
```
app_name = "MyApp"
version = "2.0-beta"
debug = false
```

**Option C: Combine both (manual merge)**
```
app_name = "MyApp"
version = "2.0"
debug = false
```

**Option D: Write something completely new**
```
app_name = "MyApp"
version = "2.1-release"
debug = false
```

The key point: **Remove all conflict markers** (`<<<<<<<`, `=======`, `>>>>>>>`) and leave only the final content you want.

### Step 4: Mark as Resolved

After editing:

```bash
# Stage the resolved file
git add config.txt

# Check status - should show "All conflicts fixed"
git status
```

### Step 5: Complete the Merge

```bash
# Commit to finish the merge
git commit -m "Merge feature branch, resolve version conflict"
```

Git may open an editor with a default merge message. You can accept it or customize it.

---

## Complete Example Walkthrough

Let's trace through a complete conflict scenario:

### Setup: Two Branches Diverge

```
main:     A ── B ── C     (C changed line 5 to "Hello World")
               \
feature:        D ── E    (E changed line 5 to "Hello Universe")
```

### Attempt the Merge

```bash
$ git switch main
$ git merge feature

Auto-merging greeting.txt
CONFLICT (content): Merge conflict in greeting.txt
Automatic merge failed; fix conflicts and then commit the result.
```

### View the Conflict

```bash
$ cat greeting.txt

Welcome to our application!

Please enjoy your stay.

<<<<<<< HEAD
Hello World
=======
Hello Universe
>>>>>>> feature

Thanks for visiting!
```

### Resolve the Conflict

Edit the file to your desired result:

```bash
$ nano greeting.txt   # or use any editor
```

After editing:
```
Welcome to our application!

Please enjoy your stay.

Hello World and Universe

Thanks for visiting!
```

### Complete the Merge

```bash
$ git add greeting.txt
$ git status

On branch main
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

$ git commit -m "Merge feature: combine greetings"

[main abc1234] Merge feature: combine greetings
```

### Final Result

```
main:     A ── B ── C ────── M    (merge commit)
               \            /
feature:        D ── E ────┘
```

---

## Types of Conflicts

### Content Conflict (Most Common)

Same lines changed differently:

```
<<<<<<< HEAD
function calculate(x) {
    return x * 2;
}
=======
function calculate(x) {
    return x * 3;
}
>>>>>>> feature
```

### Add/Add Conflict

Same filename added in both branches with different content:

```bash
CONFLICT (add/add): Merge conflict in newfile.txt
```

### Modify/Delete Conflict

File modified in one branch, deleted in another:

```bash
CONFLICT (modify/delete): config.txt deleted in feature and modified in HEAD.
```

To resolve, choose to keep the modified version or accept the deletion:

```bash
# Keep the file (your modified version)
git add config.txt

# Or accept the deletion
git rm config.txt
```

### Rename Conflicts

File renamed differently in each branch, or renamed in one and modified in another.

---

## Tools for Resolving Conflicts

### Using Git's Built-in Merge Tool

```bash
git mergetool
```

This opens a visual merge tool (if configured). You can set your preferred tool:

```bash
git config --global merge.tool vimdiff
# Or: meld, kdiff3, vscode, etc.
```

### Using VS Code

VS Code highlights conflicts and provides buttons:
- **Accept Current Change** (keep HEAD)
- **Accept Incoming Change** (keep theirs)
- **Accept Both Changes** (include both)
- **Compare Changes** (see side-by-side)

### Using Command Line Shortcuts

```bash
# Accept all changes from our branch (HEAD)
git checkout --ours filename.txt
git add filename.txt

# Accept all changes from their branch
git checkout --theirs filename.txt
git add filename.txt
```

**Warning:** These accept one side completely. Use only when you're certain you want to discard the other side entirely.

---

## Aborting a Merge

If you're in over your head, you can abort:

```bash
# Cancel the merge and go back to before you started
git merge --abort
```

This only works while the merge is in progress (before you commit).

---

## Strategies to Minimize Conflicts

| Strategy | Why It Helps |
|----------|--------------|
| **Pull frequently** | Smaller, more frequent integrations = smaller conflicts |
| **Communicate with team** | Know who's working on what |
| **Keep branches short-lived** | Less time for divergence |
| **Make small, focused commits** | Easier to understand and resolve |
| **Avoid reformatting entire files** | Don't change whitespace in lines you're not editing |
| **Use .gitattributes** | Define merge strategies for specific files |

---

## Quick Reference

### During a Conflict

```bash
# See which files have conflicts
git status

# View the conflict in a file
cat <filename>

# After editing, mark as resolved
git add <filename>

# Complete the merge
git commit

# Or abort the merge entirely
git merge --abort
```

### Conflict Markers

```
<<<<<<< HEAD
Your changes (current branch)
=======
Their changes (incoming branch)
>>>>>>> branch-name
```

### Resolution Shortcuts

```bash
# Accept current branch version for a file
git checkout --ours <file>

# Accept incoming branch version for a file  
git checkout --theirs <file>

# Then mark as resolved
git add <file>
```

---

## Key Takeaways

1. **Conflicts are normal** — They're not errors; they're Git asking for human judgment
2. **Conflict markers show both versions** — `HEAD` is yours, the branch name is theirs
3. **You decide the final content** — Remove markers and write what you want
4. **Mark resolved with `git add`** — Then commit to complete the merge
5. **You can always abort** — `git merge --abort` returns to pre-merge state
6. **Prevention beats cure** — Pull often, communicate, keep branches short

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Committing with conflict markers still in file | Always search for `<<<<<<<` before committing |
| Using `--ours` or `--theirs` without understanding | Only use when you truly want to discard one side |
| Panicking and force-pushing | Stay calm; conflicts are normal and fixable |
| Not testing after resolving | Always verify your resolution works correctly |
| Resolving without understanding both changes | Read both versions and understand the intent |

---

## Conflict Resolution Checklist

When you encounter a conflict:

- [ ] Run `git status` to see all conflicted files
- [ ] Open each conflicted file
- [ ] Find all conflict markers (`<<<<<<<`)
- [ ] Understand what each side changed and why
- [ ] Edit to create the correct final version
- [ ] Remove ALL conflict markers
- [ ] Save the file
- [ ] Run `git add <file>` for each resolved file
- [ ] Run `git status` to confirm all conflicts resolved
- [ ] Run `git commit` to complete the merge
- [ ] Test that everything works correctly

---

**Previous Module:** [Module 7 - Remote Repositories](./module-7-remote-repositories.md)  
**Next:** [Quick Reference Cheat Sheet](./cheat-sheet.md)
