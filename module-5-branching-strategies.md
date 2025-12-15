# Module 5: Branching Strategies

## Learning Objectives

By the end of this module, you will be able to:
- Explain three common branching strategies
- Compare when to use each strategy
- Execute a complete Git Flow release cycle
- Choose the right strategy for different team situations

---

## Why This Matters

A branching strategy is a team agreement about how branches are used. Without one, you get chaos: everyone does their own thing, merges conflict, and deployments become risky.

The right strategy depends on:
- Team size
- Release frequency  
- Product type (web app vs. installed software)
- Risk tolerance

---

## Key Concepts

### What is a Branching Strategy?

A branching strategy defines:
- Which branches exist and their purpose
- How code flows between branches
- When and how to create releases
- Rules for merging and integration

Think of it as "traffic rules" for your repository.

---

## Strategy 1: Trunk-Based Development

### Overview

Everyone commits to a single main branch (the "trunk") very frequently—often multiple times per day.

```
main    ●────●────●────●────●────●────●────●────●────●
              │    │              │         │    │
              └─●──┘              └──●──────┘    └─●──┘
                │                    │              │
            (feature,            (feature,      (feature,
            hours-1 day)         hours-1 day)   hours-1 day)
```

**Key characteristics:**
- Feature branches live hours to 1-2 days maximum
- Everyone integrates to main constantly
- Uses feature flags to hide incomplete work
- Requires strong automated testing

### Workflow

1. Pull latest from main
2. Create a short-lived branch (optional—some commit directly to main)
3. Make small, focused changes
4. Merge back to main within hours or 1-2 days
5. Deploy frequently (often automatically)

### Pros

| Advantage | Why It Matters |
|-----------|----------------|
| Simple mental model | One main branch to think about |
| Small, frequent merges | Fewer conflicts, easier debugging |
| Fast feedback | Problems found immediately |
| Continuous deployment | Ship features as they're ready |

### Cons

| Disadvantage | Consideration |
|--------------|---------------|
| Requires mature testing | Broken code affects everyone |
| Feature flags add complexity | Managing flags is its own challenge |
| Less structure | May not suit all teams |
| Risky without CI/CD | Manual testing can't keep up |

### Best For

- Teams with strong automated testing
- SaaS/web applications with continuous deployment
- Experienced teams comfortable with CI/CD
- Products where features can be hidden behind flags

---

## Strategy 2: GitHub Flow

### Overview

A lightweight, branch-based workflow centered on pull requests.

```
main    ●────────●─────────────────●────────────●──────────●
         \      /                 /              \        /
          ●────●                 /                ●──────●
          feature-a             /                 feature-c
                               /
                    ●────●────●
                    feature-b
```

**Key characteristics:**
- Main is always deployable
- All work happens in feature branches
- Changes go through pull requests (code review)
- Deploy after every merge to main

### Workflow

1. Create a branch from main for your feature
2. Make commits on your branch
3. Open a pull request for code review
4. Discuss and review the changes
5. Merge to main after approval
6. Deploy immediately

### Pros

| Advantage | Why It Matters |
|-----------|----------------|
| Simple and clear | Easy to learn and follow |
| Code review built-in | Pull requests encourage quality |
| Always deployable | Main branch is production-ready |
| Flexible timeline | Branches can live days or weeks |

### Cons

| Disadvantage | Consideration |
|--------------|---------------|
| No explicit release process | Just "merge and deploy" |
| Single version in production | Hard to support multiple versions |
| Long-lived branches can diverge | Encourage frequent rebasing |

### Best For

- Small to medium teams
- Web applications with single production version
- Teams that value code review
- Continuous deployment environments

---

## Strategy 3: Git Flow

### Overview

A structured branching model with specific branches for different purposes.

```
main        ●───────────────────────────────────●───────────────────●
             \                                 / \                 /
              \                               /   \               /
hotfix         \                             /     ●─────────────●
                \                           /       hotfix-1.0.1
                 \                         /
release           \                 ●─────●
                   \               /       \
                    \             /         \
develop              ●───●───────●───────────●───●───●───●
                      \     /           \         /
                       \   /             \       /
feature                 ●─●               ●─────●
                      feature-a         feature-b
```

**Branch purposes:**
- **main**: Production code only, tagged with versions
- **develop**: Integration branch, latest development work
- **feature/xxx**: Individual features, branch from develop
- **release/x.x**: Release preparation, branch from develop
- **hotfix/xxx**: Urgent production fixes, branch from main

### Workflow

1. Features branch from `develop` and merge back to `develop`
2. When ready to release, create `release/x.x` from `develop`
3. Test and polish on the release branch
4. Merge release to both `main` AND `develop`
5. Tag the release on `main`
6. Hotfixes branch from `main`, merge to both `main` and `develop`

### Pros

| Advantage | Why It Matters |
|-----------|----------------|
| Clear structure | Everyone knows which branch to use |
| Supports multiple versions | Can maintain older releases |
| Defined release process | Planned, predictable releases |
| Parallel development | Features don't block releases |

### Cons

| Disadvantage | Consideration |
|--------------|---------------|
| Complex | Many branches to understand |
| Slower iteration | More ceremony per release |
| Merge overhead | Multiple merges required |
| Overkill for small teams | Unnecessary complexity |

### Best For

- Large teams with defined roles
- Products with scheduled releases
- Software supporting multiple versions
- Projects with QA/staging environments

---

## Comparison Table

| Aspect | Trunk-Based | GitHub Flow | Git Flow |
|--------|-------------|-------------|----------|
| **Complexity** | Low | Low | High |
| **Primary Branches** | 1 (main) | 1 (main) | 2 (main, develop) |
| **Feature Branch Lifespan** | Hours to 1-2 days | Days to weeks | Days to weeks |
| **Release Process** | Continuous | Continuous | Scheduled |
| **Code Review** | Optional | Via pull requests | Via pull requests |
| **Best Team Size** | Any (with CI/CD) | Small-Medium | Medium-Large |
| **Merge Conflicts** | Rare (frequent merges) | Occasional | More common |
| **Learning Curve** | Low | Low | Medium-High |
| **Multiple Versions** | No (use feature flags) | No | Yes |

---

## Choosing a Strategy

### Choose Trunk-Based Development if:
- You have comprehensive automated testing
- You deploy multiple times per day
- Your team is experienced with CI/CD
- You can use feature flags effectively

### Choose GitHub Flow if:
- You want simplicity with code review
- You deploy after each merged pull request
- You have a small to medium team
- You support a single production version

### Choose Git Flow if:
- You have scheduled releases (sprints, versions)
- You need to support multiple release versions
- You have a larger team with dedicated QA
- You need clear separation between dev and production

---

## Exercise 5.1: Git Flow Release Cycle

Let's practice a complete Git Flow workflow: create a develop branch, work on a feature, prepare a release, and merge to main.

### Setup

```bash
# Create a new repo for this exercise
mkdir gitflow-demo
cd gitflow-demo
git init

# Create initial commit on main
echo "# My Project" > README.md
echo "Version: 0.0.0" >> README.md
git add README.md
git commit -m "Initial commit"

# Verify we're on main
git branch
```

### Step 1: Create the Develop Branch

```bash
# Create and switch to develop branch
git switch -c develop

# Update README for develop
echo "" >> README.md
echo "## Development" >> README.md
echo "This is the development branch." >> README.md

git commit -am "Set up develop branch"

# View branches
git branch
```

### Step 2: Create a Feature Branch

```bash
# Create feature branch FROM develop
git switch -c feature/add-content

# Add a new file for this feature
echo "# Content Page" > content.txt
echo "" >> content.txt
echo "Welcome to our content section." >> content.txt

git add content.txt
git commit -m "Add content page"

# Continue working on the feature
echo "" >> content.txt
echo "## Articles" >> content.txt
echo "- Article 1: Getting Started" >> content.txt
echo "- Article 2: Advanced Topics" >> content.txt

git commit -am "Add articles section to content page"

# View feature branch history
echo "=== Feature branch history ==="
git log --oneline
```

### Step 3: Merge Feature into Develop

```bash
# Switch back to develop
git switch develop

# Merge the feature
git merge feature/add-content -m "Merge feature/add-content into develop"

# Delete the feature branch (it's merged)
git branch -d feature/add-content

# View develop history
echo "=== Develop branch after feature merge ==="
git log --oneline

# Verify content is there
ls
cat content.txt
```

### Step 4: Create a Release Branch

```bash
# When ready to release, create release branch from develop
git switch -c release/1.0.0

# Update version
echo "# My Project" > README.md
echo "Version: 1.0.0" >> README.md
echo "" >> README.md
echo "## Development" >> README.md
echo "This is the development branch." >> README.md

git commit -am "Bump version to 1.0.0"

# Add release notes
echo "# Release Notes - Version 1.0.0" > RELEASE_NOTES.md
echo "" >> RELEASE_NOTES.md
echo "## Features" >> RELEASE_NOTES.md
echo "- Added content page with articles section" >> RELEASE_NOTES.md
echo "" >> RELEASE_NOTES.md
echo "## Release Date" >> RELEASE_NOTES.md
echo "$(date +%Y-%m-%d)" >> RELEASE_NOTES.md

git add RELEASE_NOTES.md
git commit -m "Add release notes for 1.0.0"

# View release branch
echo "=== Release branch history ==="
git log --oneline
```

### Step 5: Merge Release into Main (The Release!)

```bash
# Switch to main
git switch main

# Merge the release
git merge release/1.0.0 -m "Release version 1.0.0"

# Tag the release
git tag -a v1.0.0 -m "Version 1.0.0"

# View main branch
echo "=== Main branch after release ==="
git log --oneline

# View tags
echo "=== Tags ==="
git tag
```

### Step 6: Merge Release Back into Develop

```bash
# IMPORTANT: Release changes need to go back to develop
git switch develop

# Merge release into develop
git merge release/1.0.0 -m "Merge release/1.0.0 back into develop"

# Delete the release branch
git branch -d release/1.0.0

# View develop history
echo "=== Develop branch after release merge ==="
git log --oneline
```

### Step 7: View Final State

```bash
echo ""
echo "=========================================="
echo "        FINAL REPOSITORY STATE"
echo "=========================================="

# All branches
echo ""
echo "=== Branches ==="
git branch

# Main branch (production)
echo ""
echo "=== Main branch commits ==="
git log main --oneline

# Develop branch
echo ""
echo "=== Develop branch commits ==="
git log develop --oneline

# Visual graph
echo ""
echo "=== Branch graph ==="
git log --oneline --graph --all

# Tags
echo ""
echo "=== Tags (releases) ==="
git tag -n

# Files on main
echo ""
echo "=== Files on main ==="
git switch main
ls -la

echo ""
echo "=========================================="
echo "    Git Flow Release Cycle Complete!"
echo "=========================================="
```

---

## What We Practiced

| Step | Git Flow Concept |
|------|------------------|
| Create `develop` | Set up integration branch |
| Create `feature/add-content` | Isolate feature work |
| Merge feature → develop | Integrate completed feature |
| Create `release/1.0.0` | Prepare for release |
| Merge release → main | Deploy to production |
| Tag `v1.0.0` | Mark release point |
| Merge release → develop | Keep develop in sync |

---

## Quick Reference: Git Flow Commands

```bash
# Feature
git switch develop
git switch -c feature/name
# ... work ...
git switch develop
git merge feature/name
git branch -d feature/name

# Release
git switch develop
git switch -c release/x.x.x
# ... final prep ...
git switch main
git merge release/x.x.x
git tag -a vX.X.X -m "message"
git switch develop
git merge release/x.x.x
git branch -d release/x.x.x

# Hotfix (emergency production fix)
git switch main
git switch -c hotfix/description
# ... fix ...
git switch main
git merge hotfix/description
git tag -a vX.X.X -m "message"
git switch develop
git merge hotfix/description
git branch -d hotfix/description
```

---

## Key Takeaways

1. **No strategy is universally best** — Choose based on your team and project
2. **Trunk-based requires strong CI/CD** — Simple but needs infrastructure
3. **GitHub Flow balances simplicity and review** — Great for most web projects
4. **Git Flow provides structure** — Worth the complexity for scheduled releases
5. **Consistency matters most** — Pick one and stick to it

---

## Common Mistakes to Avoid

| Mistake | Solution |
|---------|----------|
| Using Git Flow for tiny projects | Use GitHub Flow instead |
| Forgetting to merge release to develop | Always merge to both main AND develop |
| Long-lived feature branches | Merge frequently or use trunk-based |
| No branch naming convention | Agree on `feature/`, `release/`, `hotfix/` prefixes |
| Skipping tags on releases | Tags make it easy to find release points |

---

**Previous Module:** [Module 4 - Branching Basics](./module-4-branching-basics.md)  
**Next Module:** [Module 6 - Cherry-Picking](./module-6-cherry-picking.md)
