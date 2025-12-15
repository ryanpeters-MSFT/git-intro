# Module 7: Remote Repositories

## Learning Objectives

By the end of this module, you will be able to:
- Explain what remote repositories are and why they matter
- Understand the relationship between local and remote branches
- Use push, pull, and fetch to synchronize with remotes
- Track remote branches and understand their status
- Clone existing repositories

---

## Why This Matters

So far, everything we've done has been local—on your own computer. But Git's real power comes from collaboration. Remote repositories let you:

- **Back up your work** to a server
- **Share code** with teammates
- **Collaborate** on the same codebase
- **Deploy** from a central location

Services like GitHub, GitLab, and Bitbucket host remote repositories, but a remote can be any Git repository accessible over a network.

---

## Key Concepts

### What is a Remote Repository?

A **remote repository** is a version of your project hosted somewhere else—usually on a server. Your local repository can connect to one or more remotes to push and pull changes.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│   YOUR COMPUTER                              SERVER (GitHub, etc.)      │
│                                                                         │
│   ┌─────────────────┐                       ┌─────────────────┐        │
│   │                 │       git push        │                 │        │
│   │  Local Repo     │ ────────────────────► │  Remote Repo    │        │
│   │                 │                       │  (origin)       │        │
│   │  - main         │ ◄──────────────────── │                 │        │
│   │  - feature      │       git pull        │  - main         │        │
│   │                 │       git fetch       │  - feature      │        │
│   └─────────────────┘                       └─────────────────┘        │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### The Default Remote: Origin

When you clone a repository or add your first remote, it's conventionally named **origin**. This is just a nickname—you can have multiple remotes with different names.

```bash
# "origin" is the conventional name for your primary remote
git remote add origin https://github.com/user/repo.git
```

### Local vs. Remote Branches

Your local repository tracks both:
- **Local branches**: `main`, `feature`, etc.
- **Remote-tracking branches**: `origin/main`, `origin/feature`, etc.

Remote-tracking branches are read-only snapshots of where the remote branches were last time you fetched.

```
Local                          Remote (origin)
─────                          ────────────────
main ────────●                 main ────────●
              \                               
origin/main ───●  (last known position of remote's main)
```

---

## Essential Remote Commands

| Command | Purpose |
|---------|---------|
| `git clone <url>` | Download a repository and set up remote |
| `git remote -v` | List configured remotes |
| `git remote add <name> <url>` | Add a new remote |
| `git fetch` | Download updates from remote (doesn't merge) |
| `git pull` | Fetch AND merge remote changes |
| `git push` | Upload local commits to remote |
| `git push -u origin <branch>` | Push and set up tracking |
| `git branch -r` | List remote-tracking branches |
| `git branch -a` | List all branches (local and remote) |

---

## Understanding Fetch vs. Pull

This is a critical distinction:

### git fetch

Downloads commits from the remote but **doesn't change your working files**:

```
Before fetch:
  Local main:    A ── B ── C
  origin/main:   A ── B         (stale - remote has moved on)
  Remote main:   A ── B ── D ── E

After fetch:
  Local main:    A ── B ── C     (unchanged!)
  origin/main:   A ── B ── D ── E   (updated)
  Remote main:   A ── B ── D ── E
```

Fetch is **safe**—it just updates your knowledge of the remote.

### git pull

Downloads commits AND merges them into your current branch:

```
Before pull:
  Local main:    A ── B ── C
  Remote main:   A ── B ── D ── E

After pull:
  Local main:    A ── B ── C ── M   (merge commit)
                      \       /
                       D ── E
```

Pull is essentially `git fetch` followed by `git merge`.

### Which Should You Use?

| Situation | Recommendation |
|-----------|----------------|
| See what's changed on remote | `git fetch` then inspect |
| Regular sync when working alone | `git pull` is fine |
| Before starting new work | `git pull` to get latest |
| Careful integration of team changes | `git fetch`, review, then merge |

---

## How Push Works

Pushing uploads your local commits to the remote:

```
Before push:
  Local main:    A ── B ── C ── D ── E
  Remote main:   A ── B ── C

After push:
  Local main:    A ── B ── C ── D ── E
  Remote main:   A ── B ── C ── D ── E
```

### Push Can Fail

If the remote has commits you don't have, Git rejects the push:

```
Local main:    A ── B ── C ── D
Remote main:   A ── B ── C ── X ── Y   (someone else pushed X and Y)

git push → REJECTED (remote has work you don't have)
```

**Solution:** Pull first (fetch + merge), then push:

```bash
git pull
# Resolve any conflicts if needed
git push
```

---

## Tracking Branches

A **tracking branch** is a local branch connected to a remote branch. This enables shortcuts:

```bash
# Without tracking, you must specify everything:
git push origin main
git pull origin main

# With tracking set up:
git push    # Git knows where to push
git pull    # Git knows where to pull from
```

### Setting Up Tracking

```bash
# When pushing a new branch for the first time:
git push -u origin feature-x
# -u (or --set-upstream) creates the tracking relationship

# Or set tracking for an existing branch:
git branch --set-upstream-to=origin/main main
```

### Checking Tracking Status

```bash
# See which branches track what:
git branch -vv

# Example output:
#   main       abc1234 [origin/main] Latest commit message
#   feature-x  def5678 [origin/feature-x: ahead 2] My feature work
```

The `[origin/main]` shows the tracking relationship. `ahead 2` means you have 2 local commits not yet pushed.

---

## Exercise 7.1: Working with a Remote Repository

**Prerequisites:** This exercise assumes you have a remote repository already created (on GitHub, GitLab, etc.) and have the URL ready.

### Setup: Connect to an Existing Remote

```bash
# Create a local repository
mkdir remote-demo
cd remote-demo
git init

# Create initial content
echo "# Remote Demo Project" > README.md
echo "Learning about Git remotes." >> README.md
git add README.md
git commit -m "Initial commit"

# Add the remote (replace with your actual URL)
# For HTTPS:
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO.git
# Or for SSH:
# git remote add origin git@github.com:YOUR-USERNAME/YOUR-REPO.git

# Verify the remote was added
git remote -v
```

### Push Your First Commits

```bash
# Push to remote and set up tracking (-u flag)
git push -u origin main

# If the remote has existing content, you might need:
# git pull --rebase origin main
# Then: git push -u origin main
```

### View Remote Information

```bash
# List remotes
git remote -v

# See detailed info about a remote
git remote show origin

# List remote-tracking branches
git branch -r

# List all branches (local and remote)
git branch -a
```

---

## Exercise 7.2: Fetch and Pull

```bash
# Make sure you're in your repository with a remote set up
cd remote-demo

# Fetch updates from the remote (safe, doesn't change your files)
git fetch origin

# See what was fetched
git log origin/main --oneline

# Compare your local main to the remote
git log main..origin/main --oneline    # Commits on remote not in local
git log origin/main..main --oneline    # Commits in local not on remote

# If there are remote changes, merge them
git merge origin/main

# Or do fetch+merge in one command
git pull
```

---

## Exercise 7.3: Push Local Changes

```bash
# Make some local changes
echo "" >> README.md
echo "## Updates" >> README.md
echo "Added new content locally." >> README.md

git commit -am "Add updates section"

# Check status relative to remote
git status
# Should show: "Your branch is ahead of 'origin/main' by 1 commit"

# Push changes to remote
git push

# Verify
git status
# Should show: "Your branch is up to date with 'origin/main'"
```

---

## Exercise 7.4: Push a New Branch

```bash
# Create a new local branch
git switch -c feature/documentation

# Add content
echo "# Documentation" > DOCS.md
echo "This is project documentation." >> DOCS.md
git add DOCS.md
git commit -m "Add documentation file"

# Push new branch to remote (with tracking)
git push -u origin feature/documentation

# List all branches including remote
git branch -a
```

---

## Exercise 7.5: Clone an Existing Repository

Cloning downloads a complete copy of a repository:

```bash
# Move out of current repo
cd ~

# Clone a repository (creates a new folder)
git clone https://github.com/SOME-USER/SOME-REPO.git

# Or clone into a specific folder name
git clone https://github.com/SOME-USER/SOME-REPO.git my-project

# Enter the cloned repo
cd my-project   # or SOME-REPO if you didn't specify

# Remote is already configured!
git remote -v

# All branches are available
git branch -a
```

---

## Common Remote Workflows

### Starting a New Project

```bash
# 1. Create repo on GitHub/GitLab (don't initialize with README)
# 2. Locally:
mkdir my-project
cd my-project
git init
echo "# My Project" > README.md
git add README.md
git commit -m "Initial commit"
git remote add origin <url>
git push -u origin main
```

### Joining an Existing Project

```bash
# 1. Clone the repository
git clone <url>
cd repo-name

# 2. Create a branch for your work
git switch -c feature/my-feature

# 3. Make changes and commit
# ... work ...
git commit -am "My changes"

# 4. Push your branch
git push -u origin feature/my-feature

# 5. Create a pull request on GitHub/GitLab
```

### Daily Workflow

```bash
# Start of day: get latest changes
git switch main
git pull

# Create branch for today's work
git switch -c feature/todays-work

# ... work and commit throughout the day ...

# End of day: push your branch
git push -u origin feature/todays-work
```

---

## Remote Branch Lifecycle

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│   1. Create local branch                                                │
│      git switch -c feature/x                                            │
│                                                                         │
│   2. Work and commit locally                                            │
│      git commit -am "Work"                                              │
│                                                                         │
│   3. Push branch to remote                                              │
│      git push -u origin feature/x                                       │
│                                                                         │
│   4. Others can see and pull the branch                                 │
│      git fetch origin                                                   │
│      git switch feature/x                                               │
│                                                                         │
│   5. Merge (usually via pull request)                                   │
│      git switch main                                                    │
│      git merge feature/x                                                │
│                                                                         │
│   6. Delete branch after merge                                          │
│      git branch -d feature/x           (local)                          │
│      git push origin --delete feature/x (remote)                        │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## Quick Reference

```bash
# Clone
git clone <url>
git clone <url> <folder-name>

# Remotes
git remote -v                    # List remotes
git remote add <name> <url>      # Add remote
git remote remove <name>         # Remove remote
git remote show <name>           # Details about remote

# Fetch (download, don't merge)
git fetch                        # All remotes
git fetch origin                 # Specific remote
git fetch origin main            # Specific branch

# Pull (fetch + merge)
git pull                         # Current tracking branch
git pull origin main             # Specific branch

# Push
git push                         # Current tracking branch
git push origin main             # Specific branch
git push -u origin <branch>      # Push and set up tracking
git push origin --delete <branch> # Delete remote branch

# Branches
git branch -r                    # Remote branches
git branch -a                    # All branches
git branch -vv                   # Show tracking info
```

---

## Key Takeaways

1. **Remotes are just URLs** — "origin" is a nickname for your primary remote
2. **Fetch is safe** — It downloads but doesn't change your working files
3. **Pull = fetch + merge** — Use when you want to integrate remote changes
4. **Push uploads your commits** — But fails if you're behind the remote
5. **Tracking branches enable shortcuts** — Set up with `push -u`
6. **Clone gives you everything** — Complete repo with history and remote configured

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Pushing to main directly | Use feature branches and pull requests |
| Force pushing (`--force`) | Almost never do this on shared branches |
| Forgetting to pull before pushing | Always pull first to check for conflicts |
| Not setting up tracking | Use `git push -u` the first time |
| Confusing fetch and pull | Fetch = download; Pull = download + merge |

---

**Previous Module:** [Module 6 - Cherry-Picking](./module-6-cherry-picking.md)  
**Next Module:** [Module 8 - Merge Conflicts](./module-8-merge-conflicts.md)
