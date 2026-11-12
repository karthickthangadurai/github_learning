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
