# gitbasics

I'll provide you with a comprehensive guide on Git commands, covering basic, branch management, and advanced commands with practical examples.

## Basic Git Commands

### `git init`
Initializes a new Git repository in the current directory.

```bash
git init
# Creates a .git folder in the current directory
```

### `git clone`
Clones an existing repository from a remote source.

```bash
git clone https://github.com/username/repository.git
# Clones the repository to your local machine

git clone https://github.com/username/repository.git my-folder
# Clones the repository into a specific folder
```

### `git status`
Shows the current state of your working directory and staging area.

```bash
git status
# Output example:
# On branch main
# Changes not staged for commit:
#   modified:   file.txt
# Untracked files:
#   newfile.js
```

### `git add`
Stages files for commit.

```bash
git add file.txt
# Stages a specific file

git add .
# Stages all changes in the current directory

git add *.js
# Stages all JavaScript files
```

### `git commit`
Commits staged changes with a message.

```bash
git commit -m "Add new feature"
# Creates a commit with the given message

git commit -am "Update file"
# Stages and commits all tracked files in one command
```

### `git push`
Uploads your commits to a remote repository.

```bash
git push
# Pushes to the default upstream branch

git push origin main
# Pushes to the 'main' branch on 'origin' remote

git push origin feature-branch
# Pushes a specific branch to remote
```

### `git pull`
Fetches and merges changes from a remote repository.

```bash
git pull
# Fetches and merges changes from the default upstream branch

git pull origin main
# Fetches and merges changes from 'main' branch on 'origin' remote

git pull --rebase
# Fetches and rebases instead of merging
```

---

## Branch Management Commands

### `git branch`
Lists, creates, or deletes branches.

```bash
git branch
# Lists all local branches
# * main
#   feature-login
#   bugfix-header

git branch -a
# Lists all local and remote branches

git branch feature-new
# Creates a new branch named 'feature-new'

git branch -d feature-old
# Deletes the 'feature-old' branch (safely)

git branch -D feature-old
# Force deletes a branch

git branch -m old-name new-name
# Renames a branch
```

### `git checkout`
Switches to a different branch or commits.

```bash
git checkout main
# Switches to the 'main' branch

git checkout -b feature-login
# Creates and switches to a new branch 'feature-login'

git checkout file.txt
# Discards changes to file.txt in the working directory
```

### `git merge`
Merges one branch into another.

```bash
git merge feature-login
# Merges 'feature-login' into the current branch (usually from main)

git merge --no-ff feature-login
# Creates a merge commit even if fast-forward is possible

git merge --squash feature-login
# Squashes all commits from 'feature-login' into one before merging
```

**Merge Conflict Resolution:**
```bash
# 1. After a merge conflict, view conflicted files
git status

# 2. Open the file and resolve conflicts manually
# 3. Stage the resolved files
git add resolved-file.txt

# 4. Complete the merge with a commit
git commit -m "Resolve merge conflicts"

# 5. Or abort the merge if needed
git merge --abort
```

### `git rebase`
Reapplies commits from one branch onto another, creating a linear history.

```bash
git rebase main
# Rebases current branch onto 'main'

git rebase -i HEAD~3
# Interactive rebase of the last 3 commits
# Allows reordering, squashing, or editing commits
```

**Interactive Rebase Options:**
```bash
# In interactive rebase editor:
# pick (p) - use commit
# reword (r) - use commit, but edit message
# squash (s) - use commit, but meld into previous
# fixup (f) - use commit, but discard log message
# drop (d) - remove commit
```

---

## Advanced Git Commands

### `git stash`
Temporarily saves changes without committing them.

```bash
git stash
# Stashes all uncommitted changes (staged and unstaged)

git stash save "Work in progress on login"
# Stashes with a descriptive message

git stash list
# Lists all stashes
# stash@{0}: WIP on main: abc1234 Add feature
# stash@{1}: Work in progress on login

git stash pop
# Applies the most recent stash and removes it

git stash apply
# Applies the most recent stash without removing it

git stash apply stash@{1}
# Applies a specific stash

git stash drop stash@{0}
# Deletes a specific stash

git stash clear
# Deletes all stashes
```

**Use Case Example:**
```bash
# You're working on feature-a when you need to switch to fix a bug
git stash
# Your changes are saved

git checkout main
git checkout -b bugfix-critical
# Fix the bug and commit

git checkout feature-a
git stash pop
# Resume your work on feature-a
```

### `git cherry-pick`
Applies specific commits from one branch to another.

```bash
git cherry-pick abc1234
# Applies the commit with hash 'abc1234' to the current branch

git cherry-pick abc1234 def5678
# Applies multiple specific commits

git cherry-pick abc1234..def5678
# Applies all commits between abc1234 and def5678 (exclusive of abc1234)

git cherry-pick abc1234^..def5678
# Applies all commits between abc1234 and def5678 (inclusive of abc1234)
```

**Handling Cherry-Pick Conflicts:**
```bash
# If conflicts occur during cherry-pick
git status
# Review the conflicts

# After resolving conflicts
git add resolved-file.txt
git cherry-pick --continue

# Or abort the cherry-pick
git cherry-pick --abort
```

**Use Case Example:**
```bash
# A critical fix was made on the develop branch
# You need this fix in production-hotfix branch

git checkout production-hotfix
git cherry-pick abc1234
# The fix is now applied to your current branch
```

### `git revert`
Creates a new commit that undoes the changes of a specific commit.

```bash
git revert abc1234
# Creates a new commit that reverses the changes of 'abc1234'

git revert -n abc1234
# Reverts without committing (allows reviewing before commit)

git revert --continue
# Completes the revert after resolving conflicts

git revert --abort
# Cancels the revert operation
```

**Use Case Example:**
```bash
# A bad commit was pushed to main
git log --oneline
# abc1234 Add buggy feature

git revert abc1234
# Creates a new commit that undoes the buggy feature
# This is safer than git reset for shared branches
```

**Differences from `git reset`:**
- `git revert` is safe for public/shared branches (creates new commits)
- `git reset` rewrites history (use only on local branches)

### `git reset`
Moves the HEAD pointer and optionally changes the staging area and working directory.

```bash
git reset HEAD~1
# Moves HEAD back one commit, keeps changes in working directory
# (Soft reset - default behavior)

git reset --soft HEAD~1
# Moves HEAD back one commit, keeps changes staged

git reset --mixed HEAD~1
# Moves HEAD back one commit, unstages changes but keeps them in working directory

git reset --hard HEAD~1
# Moves HEAD back one commit, discards all changes

git reset file.txt
# Unstages file.txt from the staging area

git reset --hard origin/main
# Discards all local changes and syncs with remote
```

**Reset Modes Explained:**

| Mode | HEAD | Staging Area | Working Directory |
|------|------|--------------|-------------------|
| `--soft` | ✓ | Unchanged | Unchanged |
| `--mixed` | ✓ | ✓ | Unchanged |
| `--hard` | ✓ | ✓ | ✓ |

**Use Case Example:**
```bash
# You committed but want to undo it and redo
git reset --soft HEAD~1
# Uncommit but keep changes staged

# Make corrections
git add corrected-file.txt
git commit -m "Fixed commit"
```

---

## Quick Reference Comparison

| Scenario | Command |
|----------|---------|
| Temporarily save work | `git stash` |
| Undo a public commit | `git revert <commit>` |
| Undo a local commit | `git reset <commit>` |
| Apply specific commits | `git cherry-pick <commit>` |
| Combine branches | `git merge` or `git rebase` |
| Create/switch branches | `git branch` / `git checkout` |
| Save work | `git commit` |
| Share work | `git push` |
| Get updates | `git pull` |

---

## Pro Tips

1. **Before destructive operations**, create a backup branch: `git branch backup`
2. **Use `git reflog`** to recover lost commits: `git reflog` shows your command history
3. **Practice on a test repository** before using advanced commands on important projects
4. **Always pull before pushing** to avoid conflicts: `git pull && git push`
5. **Use descriptive commit messages** for better history tracking
6. **Review changes before committing**: `git diff` to see unstaged changes, `git diff --staged` for staged changes

This guide should help you master Git at all levels. Practice these commands regularly to become proficient!
