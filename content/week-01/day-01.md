+++
title = "Day 01 - 15/09/2026 (On-site)"
weight = 1
+++

# GIT PRACTICE HANDBOOK — DAY 01
> Summary of common Git commands and team collaboration standards.

---

## 1. Initial Git Configuration

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git config --global user.name "Name"` | Set display name for commits | Very Frequently |
| `git config --global user.email "email"` | Set email linked to commits (should match GitHub/GitLab) | Very Frequently |
| `git config --list` | View all current Git configurations | Occasionally |
| `git config --global core.editor "code --wait"` | Set default editor for Git (VS Code) | Occasionally |

---

## 2. Project Initialization & Clone

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git init` | Initialize a new Git repository in the current folder | Occasionally |
| `git clone <url>` | Clone (download) a remote repository to local machine | Very Frequently |
| `git clone -b <branch> <url>` | Clone and checkout directly into a specific branch | Frequently |

---

## 3. Check Status & Inspect Changes

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git status` | View file statuses (modified, staged, untracked...) | Very Frequently |
| `git diff` | Compare unstaged changes with the latest commit | Frequently |
| `git diff --staged` | Compare staged changes with the latest commit | Frequently |
| `git log` | View commit history | Very Frequently |
| `git log --oneline --graph --all` | View concise commit history with branch graph | Frequently |
| `git show <commit>` | View detailed modifications of a specific commit | Occasionally |
| `git blame <file>` | See who modified each line in a file and in which commit | Occasionally |

---

## 4. Add Changes & Commit

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git add <file>` | Stage a specific file | Very Frequently |
| `git add .` | Stage all changes in the current directory | Very Frequently |
| `git commit -m "message"` | Save staged changes with a concise message | Very Frequently |
| `git commit -am "message"` | Combine `git add` (tracked files) + `commit` in one command | Frequently |
| `git commit --amend` | Modify the content/message of the latest commit (unpushed) | Occasionally |

---

## 5. Working with Branches

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git branch` | List all local branches | Very Frequently |
| `git branch <name>` | Create a new branch without switching to it | Frequently |
| `git checkout -b <name>` | Create a new branch and switch to it immediately | Very Frequently |
| `git switch -c <name>` | Similar to `checkout -b` (newer, clearer syntax) | Frequently |
| `git switch <name>` | Switch to an existing branch | Frequently |
| `git branch -d <name>` | Delete a merged branch (safe) | Occasionally |
| `git branch -D <name>` | Force delete a branch even if unmerged (use caution) | Rare / Caution |

---

## 6. Working with Remotes

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git remote -v` | View linked remote repositories (usually origin) | Frequently |
| `git remote add origin <url>` | Link a remote repository to local project | Occasionally |
| `git fetch` | Download latest changes from remote without merging | Very Frequently |
| `git pull` | Fetch and automatically merge changes from remote into current branch | Very Frequently |
| `git pull --rebase` | Fetch and rebase instead of merge (keeps commit history linear and clean) | Very Frequently |
| `git push` | Push local commits to remote | Very Frequently |
| `git push -u origin <branch>` | Push new branch to remote and set up upstream tracking | Very Frequently |
| `git push --force-with-lease` | Force push safer than `--force` (checks before overwriting) | Rare / Caution |

---

## 7. Merge & Rebase

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git merge <branch>` | Merge another branch into current branch | Very Frequently |
| `git rebase <branch>` | Reapply commits of current branch on top of another branch | Frequently |
| `git rebase -i HEAD~n` | Interactive rebase to squash/edit/drop the last n commits | Occasionally |
| `git rebase --continue` | Continue rebase process after resolving conflicts | Occasionally |
| `git rebase --abort` | Abort rebase and return to original state | Occasionally |

---

## 8. Resolving Conflicts

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git status` | View which files have conflicts that need resolution | Very Frequently |
| `git add <file>` | Mark conflict in a file as resolved | Very Frequently |
| `git merge --abort` | Abort merge process and return to pre-merge state | Occasionally |
| `git mergetool` | Open visual tool to assist with conflict resolution | Rare / Caution |

### 8.1. What is a Conflict?
A conflict occurs when two branches modify the **same lines of code** in a file, or one branch deletes a file while the other modifies it. When performing a `merge` or `rebase`, Git cannot automatically determine which code to keep and will pause the process for the developer to resolve it manually.

### 8.2. What to use for resolution? (Tools)
1. **VS Code Editor (Recommended - most intuitive):**
   * VS Code automatically detects conflicts and displays quick action buttons above the code:
     * **Accept Current Change:** Keep the code of your current branch (HEAD).
     * **Accept Incoming Change:** Take the new code from the branch being merged.
     * **Accept Both Changes:** Keep both sections of code.
     * **Resolve in Merge Editor:** Open the specialized 3-column view (Left: current code, Right: incoming code, Middle: resulting merged code).
2. **Conflict Markers (Visual indicators):**
   * If viewing the file in a standard text editor, you will see these markers:
     ```text
     <<<<<<< HEAD (Your current code)
     const apiUrl = "https://api.v1.domain.com";
     =======
     const apiUrl = "https://api.v2.domain.com";
     >>>>>>> feature/new-api (Incoming branch code)
     ```
3. **Dedicated Mergetool:**
   * Use the `git mergetool` command to open GUI software like **Beyond Compare, Meld, KDiff3, Sourcetree** when dealing with complex conflicts across many files.

### 8.3. How to resolve? (Standard step-by-step process)
* **Step 1: Identify conflicting files**
  Run this command to list the files:
  ```bash
  git status
  ```
  *(Conflicting files will be under 'Unmerged paths' with the status 'both modified').*

* **Step 2: Open files and resolve conflicts**
  * Open the affected files in VS Code.
  * Briefly discuss with the team member who also edited the file (if collaborating) to agree on the correct code.
  * Click Accept Current / Incoming / Both, or manually edit the code as needed.
  * Ensure all `<<<<<<<`, `=======`, `>>>>>>>` markers are completely removed.

* **Step 3: Mark as resolved (Stage)**
  ```bash
  git add <modified-file-name>
  # Or stage all modified files:
  git add .
  ```

* **Step 4: Complete the process**
  * If in a Merge process:
    ```bash
    git commit -m "Resolve merge conflict between main and feature branch"
    ```
  * If in a Rebase process (`git pull --rebase`):
    ```bash
    git rebase --continue
    ```
    *(Repeat Steps 2 and 3 if the next commit also has conflicts).*

> ⚠️ **Emergency Warning - Abort if you make a mistake:**
> * If merging and you mess up, run: `git merge --abort`
> * If rebasing and you want to abort to the original state, run: `git rebase --abort`

---

## 9. Stashing Changes

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git stash` | Temporarily save uncommitted changes to switch tasks/branches | Frequently |
| `git stash pop` | Restore stashed changes and remove them from stash list | Frequently |
| `git stash list` | View list of saved stashes | Occasionally |
| `git stash drop` | Delete a specific stash entry | Rare / Caution |

---

## 10. Undo & Restore

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git restore <file>` | Discard unstaged modifications in a file | Frequently |
| `git restore --staged <file>` | Unstage a file (keep local modifications) | Frequently |
| `git reset --soft HEAD~1` | Undo latest commit but keep changes staged | Occasionally |
| `git reset --hard HEAD~1` | Permanently discard latest commit and all related modifications | Rare / Caution |
| `git revert <commit>` | Create a new commit that reverses a previous commit (safe for shared branches) | Frequently |

---

## 11. Version Tagging (Tag)

| Command | Meaning | Team Usage Level |
|:---|:---|:---|
| `git tag` | List existing tags | Occasionally |
| `git tag -a v1.0 -m "message"` | Create an annotated release tag | Occasionally |
| `git push --tags` | Push all tags to remote repository | Occasionally |

---

## 12. Recommended Workflow: Feature Branch Workflow

This is the standard sequence of commands when team members start working on a new feature:

1. `git checkout dev && git pull` (Switch to dev branch and pull the latest code)
2. `git checkout -b feature/feature-name` (Create a new feature branch)
3. *... Implement code changes ...*
4. `git add . && git commit -m "Describe changes"` (Stage and commit changes)
5. `git pull --rebase origin main` (Sync with upstream to avoid conflicts)
6. `git push -u origin feature/feature-name` (Push new branch to remote)
7. Open a **Pull Request / Merge Request** on GitHub, GitLab, Bitbucket...
8. After review & approval $\rightarrow$ **Merge into main**
9. `git branch -d feature/feature-name` (Clean up local branch after merging)

---

## 13. Top Most Used Git Commands in Teams

| No. | Command |
|:---:|:---|
| 1 | `git status` |
| 2 | `git pull` |
| 3 | `git pull --rebase` |
| 4 | `git add .` |
| 5 | `git commit -m "..."` |
| 6 | `git push` |
| 7 | `git checkout -b <branch>` |
| 8 | `git merge <branch>` |
| 9 | `git log --oneline --graph --all` |
| 10 | `git stash / git stash pop` |

---

> ⚠️ **Note:** Commands labeled **"Rare / Caution"** (`git reset --hard`, `git push --force`, `git branch -D`) can cause data loss or rewrite history. Only use them when you fully understand the consequences, and avoid using them on shared branches (`main`/`develop`) where others are working.