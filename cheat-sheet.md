# Git Quick Reference Cheat Sheet

## Getting Help

```bash
git help                     # Overview of common commands
git <command> -h             # Quick help for any command
git help <command>           # Full documentation
git help -g                  # List of concept guides
```

---

## Setup & Configuration

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
git config --global init.defaultBranch main
git config --list            # View all settings
```

---

## Repository Basics

```bash
git init                     # Initialize new repository
git status                   # Check current state
git status -s                # Short status
```

---

## Staging & Committing

```bash
# Staging
git add <file>               # Stage specific file
git add .                    # Stage all in current directory
git add -A                   # Stage all changes everywhere
git restore --staged <file>  # Unstage a file

# Committing
git commit -m "message"      # Commit with message
git commit -am "message"     # Stage tracked files + commit
git commit --amend           # Modify last commit
```

---

## Viewing Changes

```bash
# Diff
git diff                     # Unstaged changes
git diff --staged            # Staged changes
git diff <commit> <commit>   # Between commits
git diff --name-only         # Just filenames

# Log
git log                      # Full history
git log --oneline            # Compact history
git log --oneline -5         # Last 5 commits
git log --oneline --graph --all  # Visual branch graph
git log -p                   # Show diffs
git log --stat               # Show file stats

# Show specific commit
git show                     # Last commit
git show <commit>            # Specific commit
```

---

## Branching

```bash
# List branches
git branch                   # Local branches
git branch -a                # All (including remote)
git branch -v                # With last commit info

# Create & switch
git branch <n>            # Create branch
git switch <n>            # Switch to branch
git switch -c <n>         # Create and switch
git switch -                  # Switch to previous branch

# Merge
git merge <branch>           # Merge into current branch
git merge <branch> -m "msg"  # With custom message

# Delete
git branch -d <n>         # Delete (if merged)
git branch -D <n>         # Force delete
```

---

## Undoing Changes

```bash
# Discard changes
git restore <file>           # Discard working directory changes
git restore .                # Discard all changes

# Unstage
git restore --staged <file>  # Unstage file
git restore --staged .       # Unstage everything

# Undo commits
git reset HEAD~1             # Undo last commit, keep changes
git reset --hard HEAD~1      # Undo last commit, discard changes
git revert <commit>          # Create new commit that undoes another
```

---

## Cherry-Picking

```bash
git cherry-pick <commit>         # Apply a single commit
git cherry-pick <c1> <c2>        # Apply multiple commits
git cherry-pick --no-commit <c>  # Apply without auto-commit
git cherry-pick --abort          # Cancel in-progress cherry-pick
git cherry-pick --continue       # Continue after resolving conflicts
```

---

## Remote Repositories

```bash
# Clone
git clone <url>                  # Clone repository
git clone <url> <folder>         # Clone into specific folder

# Remotes
git remote -v                    # List remotes
git remote add <name> <url>      # Add remote
git remote remove <name>         # Remove remote

# Fetch (download without merging)
git fetch                        # All remotes
git fetch origin                 # Specific remote

# Pull (fetch + merge)
git pull                         # Current tracking branch
git pull origin main             # Specific branch

# Push
git push                         # Current tracking branch
git push -u origin <branch>      # Push and set up tracking
git push origin --delete <branch> # Delete remote branch

# Branch tracking
git branch -r                    # List remote branches
git branch -vv                   # Show tracking info
```

---

## Merge Conflicts

```bash
# During a conflict
git status                       # See conflicted files
git merge --abort                # Cancel merge

# After resolving
git add <resolved-file>          # Mark as resolved
git commit                       # Complete merge

# Resolution shortcuts
git checkout --ours <file>       # Keep current branch version
git checkout --theirs <file>     # Keep incoming branch version
```

### Conflict Markers
```
<<<<<<< HEAD
Your changes (current branch)
=======
Their changes (incoming branch)
>>>>>>> branch-name
```

---

## Tags

```bash
git tag                      # List tags
git tag -a v1.0.0 -m "msg"   # Create annotated tag
git tag v1.0.0               # Create lightweight tag
git show v1.0.0              # Show tag details
git tag -d v1.0.0            # Delete tag
```

---

## File States

| State | Description | Command to Change |
|-------|-------------|-------------------|
| Untracked | New file, not in Git | `git add` to track |
| Staged | Ready to commit | `git commit` to save |
| Committed | Saved in history | Edit file to modify |
| Modified | Changed since last commit | `git add` to stage |

---

## Status Codes (Short Format)

| Code | Meaning |
|------|---------|
| `??` | Untracked |
| `A ` | Added (staged) |
| `M ` | Modified (staged) |
| ` M` | Modified (not staged) |
| `MM` | Modified (staged + unstaged) |
| `D ` | Deleted (staged) |
| ` D` | Deleted (not staged) |

---

## Common Workflows

### Basic Workflow
```bash
git status                   # Check state
git add <files>              # Stage changes
git commit -m "message"      # Commit
```

### Feature Branch Workflow
```bash
git switch -c feature/name   # Create feature branch
# ... make changes ...
git add .
git commit -m "Add feature"
git switch main
git merge feature/name
git branch -d feature/name
```

### Fix Last Commit
```bash
# Add forgotten file
git add forgotten-file.txt
git commit --amend

# Fix commit message
git commit --amend -m "New message"
```

---

## Tips

- Run `git status` constantly
- Commit often, commit small
- Write meaningful commit messages
- Use branches for everything
- Never force push to shared branches

---

## Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Learn Git Branching](https://learngitbranching.js.org/)
- [Oh Shit, Git!?!](https://ohshitgit.com/)
