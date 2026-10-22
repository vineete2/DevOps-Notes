# Basic Git commands: definition, syntax, example, use case, interview Q&A

## 1. `git init`
**Definition:** Creates a new, empty Git repository in the current folder by adding a hidden `.git` directory.
**Syntax:** `git init [directory]`
**Example:**
```bash
mkdir myproject && cd myproject
git init
```
**Use case:** Starting version control on a brand-new project.
**Interview answer:** "`git init` turns a normal folder into a Git repository. It creates the `.git` directory, which stores all history, branches, and config. I use it when starting a project locally before connecting it to a remote like GitHub."

## 2. `git clone`
**Definition:** Copies an existing remote repository, with its full history, to your machine.
**Syntax:** `git clone <url> [folder-name]`
**Example:**
```bash
git clone https://github.com/user/repo.git
```
**Use case:** Joining an existing project, or getting your own working copy of a repo.
**Interview answer:** "`git clone` downloads the entire repository, including all commits and branches, and automatically sets up `origin` as the remote. The difference from `init` is that `clone` starts from an existing repo, while `init` starts from scratch."

## 3. `git status`
**Definition:** Shows the state of your working directory and staging area: modified, staged, and untracked files.
**Syntax:** `git status` (or `git status -s` for short output)
**Example:**
```bash
git status
# Changes not staged for commit: modified: app.py
# Untracked files: notes.txt
```
**Use case:** Checking what changed before you `add` or `commit`.
**Interview answer:** "It's the command I run most often. It tells me which files are untracked, modified, or staged, and which branch I'm on, so I never commit something by accident."

## 4. `git add`
**Definition:** Moves changes from the working directory to the staging area, marking them for the next commit.
**Syntax:** `git add <file>` | `git add .` | `git add -p`
**Example:**
```bash
git add app.py
git add .        # stage everything
git add -p       # stage selected chunks interactively
```
**Use case:** Choosing exactly which changes go into a commit.
**Interview answer:** "Git has a staging area between the working directory and the repository. `git add` puts changes there so I can build clean, focused commits instead of committing everything at once."

## 5. `git commit`
**Definition:** Saves the staged changes as a snapshot in the local repository history, with a message.
**Syntax:** `git commit -m "message"` | `git commit --amend`
**Example:**
```bash
git commit -m "Add login validation"
git commit --amend -m "Fix typo in message"   # edit last commit
```
**Use case:** Recording a logical unit of work, such as a feature or bug fix.
**Interview answer:** "A commit is a snapshot of the staged changes with a unique SHA, an author, a timestamp, and a message. Commits are local until I push. I write small, meaningful commits with clear messages so history is easy to review and revert."

## 6. `git log`
**Definition:** Shows the commit history of the current branch.
**Syntax:** `git log [options]`
**Example:**
```bash
git log
git log --oneline --graph --all
git log -n 5
git log --author="prasanth"
```
**Use case:** Finding who changed what and when, or finding a commit hash for reset/revert.
**Interview answer:** "`git log` lists commits with hash, author, date, and message. I usually use `--oneline --graph` for a compact view of branches and merges."

## 7. `git push`
**Definition:** Uploads your local commits to a remote repository.
**Syntax:** `git push <remote> <branch>`
**Example:**
```bash
git push origin main
git push -u origin feature-login   # set upstream on first push
```
**Use case:** Sharing your work with the team or backing it up on GitHub/GitLab.
**Interview answer:** "`git push` sends my local commits to the remote branch. If the remote has commits I don't have, the push is rejected and I need to pull first. I avoid `--force` on shared branches because it rewrites history. If I must, I use `--force-with-lease`."

## 8. `git reset`
**Definition:** Moves the current branch pointer to an earlier commit, optionally changing the staging area and working directory. It **rewrites history**.
**Syntax:** `git reset [--soft | --mixed | --hard] <commit>`

| Mode | Commit history | Staging area | Working files |
|---|---|---|---|
| `--soft` | Moved back | Kept | Kept |
| `--mixed` (default) | Moved back | Cleared | Kept |
| `--hard` | Moved back | Cleared | **Deleted** |

**Example:**
```bash
git reset --soft HEAD~1    # undo last commit, keep changes staged
git reset --hard HEAD~1    # undo last commit and discard changes
git reset file.txt         # unstage a file
```
**Use case:** Undoing local commits that haven't been pushed, or unstaging files.
**Interview answer:** "`reset` moves HEAD and the branch pointer backwards. `--soft` keeps changes staged, `--mixed` keeps them unstaged, and `--hard` discards them. I only use it on local, unpushed commits because it rewrites history."

## 9. `git revert`
**Definition:** Creates a **new commit** that undoes the changes of a previous commit, without deleting history.
**Syntax:** `git revert <commit-hash>`
**Example:**
```bash
git revert a1b2c3d
git revert HEAD
```
**Use case:** Undoing a bad commit that is already pushed to a shared branch.
**Interview answer:** "`revert` is the safe undo. It adds a new commit that reverses an earlier one, so history is preserved and teammates aren't affected. `reset` rewrites history, so I use `revert` for pushed commits and `reset` for local ones."

## 10. `git clean`
**Definition:** Permanently deletes **untracked** files from the working directory.
**Syntax:** `git clean [-n] [-f] [-d] [-x]`
**Example:**
```bash
git clean -n      # dry run: show what would be deleted
git clean -f      # delete untracked files
git clean -fd     # also delete untracked directories
```
**Use case:** Removing build artifacts, temp files, or leftovers to get a clean working tree.
**Interview answer:** "`git clean` removes untracked files, which Git doesn't track and therefore can't recover. I always run `git clean -n` first as a dry run to see what will be deleted."

## 11. `git merge`
**Definition:** Combines the changes of one branch into the current branch.
**Syntax:** `git merge <branch>`
**Example:**
```bash
git checkout main
git merge feature-login
git merge --no-ff feature-login   # always create a merge commit
```
**Use case:** Integrating a finished feature or bug-fix branch into `main`.
**Interview answer:** "`git merge` joins two branches' histories. If the target branch hasn't diverged, it does a fast-forward. Otherwise it creates a merge commit. If both branches edited the same lines, I get a merge conflict that I resolve manually, then `git add` and `git commit`."

## Typical workflow
```bash
git clone <url>                 # or git init
git status
git add .
git commit -m "Add feature"
git log --oneline
git push origin main
```

## Common interview comparisons
- **reset vs revert:** `reset` rewrites history (local only). `revert` adds a new undo commit (safe for shared branches).
- **clone vs init:** `clone` copies an existing repo. `init` creates a new one.
- **add vs commit:** `add` stages changes. `commit` records them permanently.
- **commit vs push:** `commit` saves locally. `push` shares with the remote.
- **reset --hard vs clean:** `reset --hard` discards changes to *tracked* files. `clean` removes *untracked* files.
