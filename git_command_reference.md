# Git Command Reference (cumulative)

Each entry: Command, Purpose, Why, Syntax, Example, Expected Output, Real-world Scenario, Common Mistakes, Related Commands.

---

## git --version

**Command:**
```bash
git --version
```
**Purpose:** Shows which version of Git is installed.
**Why do we need it?** Confirms Git is installed and tells you if it is too old for a feature.
**Syntax:** `git --version`
**Example:** `git --version`
**Expected Output:** `git version 2.49.0`
**Real-world Scenario:** Setting up a new laptop, or debugging a teammate's "command not found" error.
**Common Mistakes:** Typing `git -version` (single dash) — it needs two dashes.
**Related Commands:** `git config --list`

---

## git config

**Command:**
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```
**Purpose:** Sets your identity (and other settings) that Git stamps on every commit.
**Why do we need it?** Every commit records an author. Without this, Git can't attribute your work.
**Syntax:** `git config [--global|--local] <key> [<value>]`. `--global` applies to all repos for your user; `--local` to the current repo only.
**Example:** `git config --global user.name` (reads the value)
**Expected Output:** `karthickthangadurai`
**Real-world Scenario:** Using a work email in company repos and a personal email in side projects (use `--local` in the work repo).
**Common Mistakes:** Using an email that doesn't match your GitHub account, so commits aren't linked to your profile.
**Related Commands:** `git config --list`

---

## git init

**Command:**
```bash
git init -b main
```
**Purpose:** Turns the current folder into a Git repository by creating a hidden `.git` folder.
**Why do we need it?** Git tracks nothing until you initialize a repo. This is step one for any new project.
**Syntax:** `git init [-b <branch-name>] [<directory>]`. `-b` names the first branch.
**Example:** `mkdir ai-notes && cd ai-notes && git init -b main`
**Expected Output:** `Initialized empty Git repository in .../ai-notes/.git/`
**Real-world Scenario:** Starting a brand-new project locally before pushing it to GitHub.
**Common Mistakes:** Running it inside an existing repo (creates a nested repo), or in your home folder (tracks everything). Always `cd` into the project first.
**Related Commands:** `git clone` (copy an existing repo instead), `git status`

---

## git status

**Command:**
```bash
git status
```
**Purpose:** Shows the state of your working directory and staging area.
**Why do we need it?** It tells you what changed, what is staged, and what is untracked. Run it constantly.
**Syntax:** `git status [-s]` (`-s` for short format)
**Example:** `git status`
**Expected Output:** `On branch main` / `nothing to commit, working tree clean`, or lists of untracked, modified and staged files.
**Real-world Scenario:** Before every commit, to confirm exactly what you're about to save.
**Common Mistakes:** Assuming "untracked" files will be committed. They are not until you `git add` them.
**Related Commands:** `git add`, `git diff`, `git log`

---

## git add

**Command:**
```bash
git add README.md
```
**Purpose:** Moves changes from the working directory into the staging area.
**Why do we need it?** It lets you choose exactly which changes go into the next commit.
**Syntax:** `git add <file>...`, `git add <folder>/`, `git add .` (everything in the current folder)
**Example:** `git add README.md`
**Expected Output:** No output on success. `git status` then shows "Changes to be committed".
**Real-world Scenario:** You fixed a bug and also experimented. You stage only the bug-fix files so the commit stays focused.
**Common Mistakes:** Using `git add .` blindly, which can stage secrets (`.env`) or large files. Check `git status` first.
**Related Commands:** `git status`, `git restore --staged` (unstage), `git commit`

---

## git commit

**Command:**
```bash
git commit -m "Add README"
```
**Purpose:** Saves the staged changes as a permanent snapshot in the local repository.
**Why do we need it?** Commits are the units of history you can review, revert or share.
**Syntax:** `git commit -m "<message>"`
**Example:** `git commit -m "Add README"`
**Expected Output:** `[main (root-commit) 2477377] Add README` then `1 file changed, 3 insertions(+)`
**Real-world Scenario:** After finishing one small logical change (a fix, a feature step), you commit it with a clear message.
**Common Mistakes:** Forgetting to `git add` first ("nothing added to commit"); vague messages like "update" or "fix".
**Related Commands:** `git add`, `git log`, `git commit --amend`

---

## git log

**Command:**
```bash
git log
```
**Purpose:** Lists the commit history, newest first.
**Why do we need it?** You find who changed what and when, and get hashes for undoing or comparing.
**Syntax:** `git log [--oneline] [-n <count>]`
**Example:** `git log`
**Expected Output:** `commit <hash>`, `Author: ...`, `Date: ...`, then the message.
**Real-world Scenario:** Accuracy dropped; you scan recent commits to find which change caused it.
**Common Mistakes:** Getting stuck in the pager on long logs. Press `q` to quit.
**Related Commands:** `git diff`, `git show`, `git reflog`

---

## git diff

**Command:**
```bash
git diff
git diff --staged
```
**Purpose:** Shows line-by-line what changed. Plain `git diff` shows unstaged changes; `--staged` shows staged changes.
**Why do we need it?** To review your own changes before committing and avoid committing mistakes or secrets.
**Syntax:** `git diff [--staged] [<file>]`
**Example:** `git diff prompt.txt`
**Expected Output:** Lines starting with `-` (removed) and `+` (added), with `@@` hunk headers.
**Real-world Scenario:** Before committing a prompt change, you check exactly which lines moved.
**Common Mistakes:** Running `git diff` after `git add` and seeing nothing. Use `--staged` then.
**Related Commands:** `git status`, `git add`, `git log -p`

---

## git restore --staged

**Command:**
```bash
git restore --staged progress_tracker.md
```
**Purpose:** Unstages a file. It goes back to the working directory with your edits intact.
**Why do we need it?** You staged a file by mistake and want it out of the next commit.
**Syntax:** `git restore --staged <file>`
**Example:** `git restore --staged progress_tracker.md`
**Expected Output:** None. `git status` shows the file as modified, not staged.
**Real-world Scenario:** After `git add .` you notice `.env` was staged; you unstage it before committing.
**Common Mistakes:** Leaving out `--staged`. `git restore <file>` alone DISCARDS your edits (destructive).
**Related Commands:** `git add`, `git status`, `git reset`

---

## git restore (without --staged)  [DESTRUCTIVE]

**Command:**
```bash
git restore config.txt
```
**Purpose:** Discards your uncommitted edits in a file, restoring it to the last commit.
**Why do we need it?** To throw away an experiment that went wrong.
**Syntax:** `git restore <file>`
**Expected Output:** None. The file reverts.
**Real-world Scenario:** You changed hyperparameters, they were bad, you reset the file.
**Common Mistakes:** Running it on a file with edits you wanted. Uncommitted edits cannot be recovered. Check `git diff` first.
**Related Commands:** `git diff`, `git restore --staged`, `git stash`

---

## git reset

**Command:**
```bash
git reset --soft HEAD~1
```
**Purpose:** Moves the branch back to an earlier commit. `--soft` keeps changes staged, default (mixed) keeps them unstaged, `--hard` DISCARDS them.
**Why do we need it?** To undo or redo a commit made too early or with a bad message.
**Syntax:** `git reset [--soft|--mixed|--hard] <commit>`
**Expected Output:** `--soft`/mixed: none. `--hard`: `HEAD is now at <hash> <message>`
**Real-world Scenario:** You committed too soon; reset --soft, add more, recommit.
**Common Mistakes:** `--hard` loses uncommitted work. Never reset commits you already pushed and shared.
**Related Commands:** `git revert` (safe undo for shared commits), `git reflog`

---

## git commit -a / -am

**Command:**
```bash
git commit -am "Fix typo"
```
**Purpose:** Stages all modified tracked files and commits them in one step (`-a` = all tracked changes, `-m` = message).
**Why do we need it?** A shortcut when every modified file belongs in the commit.
**Syntax:** `git commit -a -m "<message>"` or `git commit -am "<message>"`
**Example:** `git commit -am "Fix typo"`
**Expected Output:** `[main <hash>] Fix typo` and a file count.
**Real-world Scenario:** A quick one-file fix where you know nothing else changed.
**Common Mistakes:** It commits EVERY modified tracked file, including ones you did not mean to include. It skips untracked (new) files. Check `git status -s` first.
**Related Commands:** `git add`, `git status`, `git reset --soft HEAD~1` (to undo)

---

# Flags Cheat Sheet

A **flag** (also called an option) changes how a command behaves. Conventions:
- **Short flag**: one dash, one letter (`-m`). Short flags can be combined: `-am` = `-a -m`.
- **Long flag**: two dashes, a word (`--staged`).
- Many flags take a value right after them: `-m "message"`, `-b main`.
- Many have both forms; they do the same thing (`-s` = `--short`).

## Flags used so far

| Flag | Used with | Meaning | Example |
|---|---|---|---|
| `-m "msg"` | `git commit` | Message for the commit, written inline. Without it Git opens an editor. | `git commit -m "Add README"` |
| `-a` | `git commit` | Stage all modified **tracked** files first. Skips untracked files. Commits everything modified, so check first. | `git commit -a -m "Fix"` |
| `-am "msg"` | `git commit` | `-a` and `-m` combined. | `git commit -am "Fix typo"` |
| `-b <name>` | `git init` | Name of the first branch. Without it Git uses its default (often `master`). | `git init -b main` |
| `-s` / `--short` | `git status` | Compact two-column output. First column = staged, second = unstaged. | `git status -s` |
| `--oneline` | `git log` | One line per commit: short hash and message. | `git log --oneline` |
| `--staged` (also `--cached`) | `git diff` | Show changes that are staged (what the next commit contains). | `git diff --staged` |
| `--staged` | `git restore` | Unstage a file; keep your edits. Without it `restore` DISCARDS edits. | `git restore --staged file.md` |
| `--stat` | `git diff`, `git show` | Summary of files changed and line counts, not the full diff. | `git diff --staged --stat` |
| `--soft` | `git reset` | Undo commits, keep changes **staged**. Safe. | `git reset --soft HEAD~1` |
| `--mixed` (default) | `git reset` | Undo commits, keep changes **unstaged**. Safe. | `git reset HEAD~1` |
| `--hard` | `git reset` | Undo commits AND discard changes. DESTRUCTIVE. | `git reset --hard HEAD~1` |
| `--global` | `git config` | Apply the setting to all repos for your user. | `git config --global user.name "Name"` |
| `--local` | `git config` | Apply the setting only to the current repo. (default) | `git config --local user.email "x@y.com"` |
| `--version` | `git` | Print the installed Git version. | `git --version` |

## `-b` has different meanings in different commands

- `git init -b main` = name the initial branch `main`.
- Later (branching lessons): `git checkout -b feature-x` = create a new branch and switch to it. The modern equivalent is `git switch -c feature-x` (`-c` = create).
- Don't assume a flag means the same thing everywhere. Check `git <command> --help`.

## Flags coming in later lessons (preview)

| Flag | Command | Meaning |
|---|---|---|
| `-u` / `--set-upstream` | `git push` | Link your local branch to a remote branch (first push) |
| `-d` / `-D` | `git branch` | Delete a branch (`-D` force-deletes unmerged) |
| `-p` | `git add`, `git log` | Patch mode: pick individual changes / show diffs in the log |
| `--amend` | `git commit` | Rewrite the last commit |
| `--no-ff` | `git merge` | Always create a merge commit |
| `--force-with-lease` | `git push` | Safer forced push |
| `-n <count>` / `-<count>` | `git log` | Limit to the last N commits (`git log -3`) |
| `--graph` | `git log` | Draw the branch structure in text |

## Tip: help is built in

```bash
git commit --help      # full manual in your pager (q to quit)
git commit -h          # short usage summary in the terminal
```

---

## git remote

**Command:**
```bash
git remote add origin https://github.com/<user>/<repo>.git
git remote -v
```
**Purpose:** Manages links to remote repositories. `add` creates a nickname (`origin`) for a URL; `-v` lists them.
**Why do we need it?** A local repo only syncs with GitHub if it knows where the remote is.
**Syntax:** `git remote add <name> <url>`, `git remote -v`
**Expected Output (`-v`):** `origin  <url> (fetch)` and `origin  <url> (push)`
**Real-world Scenario:** You started a project locally and later created the GitHub repo.
**Common Mistakes:** Wrong URL; adding `origin` twice ("remote origin already exists").
**Related Commands:** `git push`, `git clone`

---

## git push

**Command:**
```bash
git push -u origin main   # first time
git push                  # afterwards
```
**Purpose:** Uploads your local commits to the remote branch.
**Why do we need it?** Backup and sharing. Teammates can't see commits that exist only on your machine.
**Syntax:** `git push [-u] [<remote> <branch>]`. `-u` links the local branch to the remote one.
**Expected Output:** `main -> main` with object counts.
**Real-world Scenario:** End of a work session or when a PR branch is ready for review.
**Common Mistakes:** Rejected push when the remote is ahead (pull first). Never use `--force` on shared branches.
**Related Commands:** `git pull`, `git status -sb`

---

## git fetch

**Command:**
```bash
git fetch
```
**Purpose:** Downloads new commits from the remote into `origin/<branch>` without changing your files.
**Why do we need it?** Safe way to see what changed remotely before merging.
**Syntax:** `git fetch [<remote>]`
**Expected Output:** `f43bbbc..1d5c4bd  main -> origin/main`
**Real-world Scenario:** Checking whether a teammate pushed before you start your day.
**Common Mistakes:** Thinking fetch updates your files. It doesn't; you still need merge or pull.
**Related Commands:** `git pull`, `git diff HEAD origin/main`, `git log origin/main`

---

## git pull

**Command:**
```bash
git pull
```
**Purpose:** `git fetch` plus merging the remote branch into your current branch.
**Why do we need it?** Keeps your local branch up to date with teammates' work.
**Syntax:** `git pull [<remote> <branch>]`
**Expected Output:** `Fast-forward` and a file change summary.
**Real-world Scenario:** Start of every work session.
**Common Mistakes:** Pulling with uncommitted edits that touch the same files (commit or stash first); unexpected merge conflicts.
**Related Commands:** `git fetch`, `git merge`, `git status -sb`
