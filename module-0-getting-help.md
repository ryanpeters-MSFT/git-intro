# Module 0: Getting Help in Git

## Learning Objectives

By the end of this module, you will be able to:
- Use Git's built-in help system to find information about any command
- Understand the difference between brief and detailed help
- Know where to find additional documentation

---

## Why This Matters

Git has over 150 commands and countless options. Nobody memorizes them all. The key skill is knowing **how to find help quickly**. Git's built-in help system is comprehensive and always available—even offline.

---

## Key Concepts

### The Help System Hierarchy

Git provides help at multiple levels of detail:

| Command | What It Shows |
|---------|---------------|
| `git help` | Overview of common commands by category |
| `git <command> -h` | Quick summary of command options (in terminal) |
| `git help <command>` | Full manual page with detailed explanations |
| `git help -a` | List of ALL available Git commands |
| `git help -g` | List of concept guides (tutorials) |

---

## Help Commands Explained

### 1. `git help` — Command Overview

Shows a categorized list of the most common Git commands.

```bash
git help
```

**Output includes:**
- **Start a working area:** `clone`, `init`
- **Work on current change:** `add`, `mv`, `restore`, `rm`
- **Examine history:** `log`, `diff`, `show`, `status`
- **Grow/mark history:** `branch`, `commit`, `merge`, `tag`
- **Collaborate:** `fetch`, `pull`, `push`

**When to use:** When you can't remember the name of a command but know what you want to do.

---

### 2. `git <command> -h` — Quick Reference

Shows a brief summary of a command's options directly in your terminal. This is the fastest way to get help.

```bash
git status -h
git commit -h
git branch -h
git log -h
```

**Example output for `git commit -h`:**

```
usage: git commit [-a | --interactive | --patch] [-s] [-v] [-u<mode>]
                  [--amend] [--dry-run] [(-c | -C | --squash) <commit>]
                  [-F <file> | -m <msg>] ...

    -q, --quiet           suppress summary after successful commit
    -v, --verbose         show diff in commit message template
    -a, --all             commit all changed files
    -m, --message <msg>   commit message
    --amend               amend previous commit
    ...
```

**When to use:** When you know the command but need to quickly check an option or flag.

---

### 3. `git help <command>` — Full Documentation

Opens the complete manual page for a command. This includes detailed explanations, all options, and often examples.

```bash
git help commit
git help branch
git help log
```

**Alternatives (all do the same thing):**
```bash
git help commit
git commit --help
man git-commit
```

**When to use:** When you need to understand a command deeply, or when the `-h` output isn't enough.

**Tip:** Press `q` to quit the manual page, use arrow keys or Page Up/Down to scroll, and press `/` to search within the page.

---

### 4. `git help -a` — All Commands

Lists every Git command available on your system.

```bash
git help -a
```

This shows:
- Main porcelain commands (everyday use)
- Ancillary commands (helpers)
- Low-level plumbing commands (internal operations)

**When to use:** When you're looking for a command that might exist but isn't in the common list.

---

### 5. `git help -g` — Concept Guides

Lists tutorial-style guides that explain Git concepts rather than specific commands.

```bash
git help -g
```

**Available guides include:**
- `git help tutorial` — A tutorial introduction to Git
- `git help everyday` — Everyday Git commands
- `git help workflows` — Overview of recommended workflows
- `git help glossary` — Git terminology explained
- `git help revisions` — How to specify revisions (commits)

**To read a guide:**
```bash
git help tutorial
git help glossary
```

**When to use:** When you want to understand concepts rather than just command syntax.

---

## Exercise 0.1: Explore the Help System

Try each of these commands and observe the output:

```bash
# See the overview of common commands
git help

# Get quick help for the status command
git status -h

# Get quick help for the add command
git add -h

# See the full documentation for commit
git help commit
# (Press 'q' to exit)

# List all available commands
git help -a

# List available concept guides
git help -g

# Read the glossary of Git terms
git help glossary
# (Press 'q' to exit)
```

---

## Exercise 0.2: Find Answers Using Help

Use the help system to answer these questions:

1. **What flag do you use with `git commit` to amend the previous commit?**
   ```bash
   git commit -h
   # Look for "amend" in the output
   ```

2. **What does the `-p` flag do with `git add`?**
   ```bash
   git add -h
   # Look for "-p" in the output
   ```

3. **How do you delete a branch?**
   ```bash
   git branch -h
   # Look for "delete" in the output
   ```

4. **What command shows the commit history as a graph?**
   ```bash
   git log -h
   # Look for "graph" in the output
   ```

---

## Quick Reference

| What You Need | Command |
|---------------|---------|
| Overview of common commands | `git help` |
| Quick syntax reminder | `git <command> -h` |
| Full documentation | `git help <command>` |
| List all commands | `git help -a` |
| Concept tutorials | `git help -g` |
| Specific guide | `git help <guide-name>` |

---

## Key Takeaways

1. **`-h` is your friend** — Use `git <command> -h` for quick, in-terminal help
2. **Full docs are always available** — Use `git help <command>` for comprehensive information
3. **Guides explain concepts** — Use `git help -g` to find tutorials on workflows and concepts
4. **You don't need to memorize everything** — Knowing how to find help is more valuable than memorizing flags

---

**Next Module:** [Module 1 - Quickstart: Your First Repository](./module-1-quickstart.md)
