# Git Fundamentals Workshop

A hands-on introduction to version control with Git.

**Level:** Beginner  
**Prerequisites:** Basic command line knowledge, text editor installed

---

## Workshop Modules

| Module | Topic | Description |
|--------|-------|-------------|
| [Module 0](./module-0-getting-help.md) | Getting Help | Using Git's built-in help system |
| [Module 1](./module-1-quickstart.md) | Quickstart | Your first repository and commit |
| [Module 2](./module-2-three-areas.md) | Three Areas | Working Directory, Staging Area, Repository |
| [Module 3](./module-3-core-commands.md) | Core Commands | status, log, diff, commit |
| [Module 4](./module-4-branching-basics.md) | Branching Basics | Create, switch, merge branches |
| [Module 5](./module-5-branching-strategies.md) | Branching Strategies | Trunk-Based, GitHub Flow, Git Flow |
| [Module 6](./module-6-cherry-picking.md) | Cherry-Picking | Apply specific commits to other branches |
| [Module 7](./module-7-remote-repositories.md) | Remote Repositories | Push, pull, fetch, and clone |
| [Module 8](./module-8-merge-conflicts.md) | Merge Conflicts | Understanding and resolving conflicts |
| [Cheat Sheet](./cheat-sheet.md) | Quick Reference | All commands in one place |

---

## Quick Setup

Before starting, configure Git with your identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

Verify your setup:

```bash
git config --list
```

---

## How to Use This Workshop

### Self-Paced Learning
Work through each module in order. Each module builds on the previous one.

### Instructor-Led
The instructor will guide you through each module, demonstrating concepts before exercises.

### Reference
Use the modules and cheat sheet as reference material after completing the workshop.

---

## Learning Path

```
Module 0: Getting Help
        │
        ▼
Module 1: Quickstart ──────► You can make commits!
        │
        ▼
Module 2: Three Areas ─────► You understand the mental model
        │
        ▼
Module 3: Core Commands ───► You can work effectively day-to-day
        │
        ▼
Module 4: Branching Basics ► You can work with branches
        │
        ▼
Module 5: Strategies ──────► You can choose a team workflow
        │
        ▼
Module 6: Cherry-Picking ──► You can apply specific commits
        │
        ▼
Module 7: Remotes ─────────► You can collaborate with others
        │
        ▼
Module 8: Merge Conflicts ─► You can handle integration issues
```

---

## What You'll Learn

By the end of this workshop, you will be able to:

- ✅ Use Git's help system to find information
- ✅ Initialize repositories and make commits
- ✅ Understand Git's three areas (working directory, staging, repository)
- ✅ Use essential commands: status, log, diff, add, commit
- ✅ Create, switch, and merge branches
- ✅ Compare branching strategies (Trunk-Based, GitHub Flow, Git Flow)
- ✅ Execute a complete release workflow
- ✅ Cherry-pick specific commits between branches
- ✅ Work with remote repositories (push, pull, fetch, clone)
- ✅ Understand and resolve merge conflicts

---

## What We Don't Cover (Advanced Topics)

- Rebasing and interactive rebase
- Advanced history manipulation (`reflog`, `bisect`)
- Git hooks
- Submodules
- Worktrees

---

## Tips for Success

1. **Type the commands yourself** — Don't just read; practice
2. **Use `git status` constantly** — It's your best friend
3. **Don't be afraid to experiment** — Create a test repo and try things
4. **Read the output** — Git usually tells you what to do next
5. **Ask for help** — Use `git <command> -h` when stuck

---

## Additional Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [GitHub Skills](https://skills.github.com/) — Interactive tutorials
- [Learn Git Branching](https://learngitbranching.js.org/) — Visual learning
- [Oh Shit, Git!?!](https://ohshitgit.com/) — Fixing common mistakes
- [Conventional Commits](https://www.conventionalcommits.org/) — Message standards

---

## Feedback

Found an issue or have suggestions? Please provide feedback to help improve this workshop.

---

*Happy Git-ing!* 🎉
