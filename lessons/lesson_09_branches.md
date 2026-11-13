# Lesson 9: Branches

**Scenario:** `main` must always work. To try a new feature safely, you create a branch, work there, and merge later.

## Concepts
- **Branch** = a movable pointer to a commit; an isolated line of development. Cheap to create.
- **main** = the stable, shared branch.
- **Feature branch** = short-lived branch for one task (`feature-greeting`, `fix-readme-typo`).
- **HEAD** = which branch/commit you are on now. Switching branches changes your files.

## Demo (throwaway repo)
```bash
git init -b main
echo "v1" > app.py && git add . && git commit -m "Add app"
git branch                         # * main
git switch -c feature-greeting     # create and switch
git branch                         # * feature-greeting / main
echo "def greet(): pass" >> app.py
git commit -am "Add greet function"
git switch main
cat app.py                         # only "v1": feature work is not on main
git log --oneline --graph --all    # see all branches
```

## Commands
| Command | Purpose |
|---|---|
| `git branch` | list local branches (`*` = current) |
| `git branch <name>` | create a branch (does not switch) |
| `git switch <name>` | switch to an existing branch |
| `git switch -c <name>` | create and switch (`-c` = create) |
| `git log --oneline --graph --all` | draw all branches |
| `git push -u origin <name>` | publish the branch to GitHub (first time) |

Older equivalent: `git checkout -b <name>` (create+switch) and `git checkout <name>`. Both work; `switch` is the modern, clearer one.

## Naming
Lowercase, hyphens, describe the task: `feature-login`, `fix-typo-readme`, `docs-add-usage`. No spaces.

## Safety
- Commit or stash before switching, or Git may refuse if your edits would be overwritten.
- Check `git branch` before you start editing, so you don't commit on `main` by accident.

## Exercise 9 (in `github_learning`)
```bash
git status -sb
git switch -c add-usage-section
git branch
echo "" >> README.md
echo "## Usage" >> README.md
echo "Notes on practicing Git commands." >> README.md
git diff
git add README.md
git commit -m "Add usage section to README"
git log --oneline --graph --all
git switch main
cat README.md
git switch add-usage-section
cat README.md
git status -sb
```
Expected: on `main`, README lacks the Usage section; on the branch, it has it.

Questions:
1. Why did `README.md` change when you switched branches?
2. Which command creates a branch and switches to it in one step?
3. Why is it better to work on a branch than directly on `main`?

Do NOT merge or push yet. Merging is the next lesson.

## Exercise 9: every command explained

| Command | What it does | Why you run it here |
|---|---|---|
| `git status -sb` | Short status plus branch info (`## main...origin/main`) | Confirm you start on `main` with a clean tree |
| `git switch -c add-usage-section` | Creates branch `add-usage-section` and moves you onto it. `-c` = create | All new work goes on its own branch, not `main` |
| `git branch` | Lists local branches; `*` marks the current one | Verify you are on the new branch before editing |
| `echo "" >> README.md` | Appends an empty line. `>>` appends, `>` would overwrite | Gives a blank line before the new heading |
| `echo "## Usage" >> README.md` | Appends the text `## Usage` (a Markdown heading) | The change we want to commit |
| `echo "Notes on practicing Git commands." >> README.md` | Appends a sentence | Same |
| `git diff` | Shows unstaged changes, `+` = added | Review your own change before staging |
| `git add README.md` | Stages that one file | Choose exactly what goes in the commit |
| `git commit -m "Add usage section to README"` | Saves the staged snapshot on the CURRENT branch | The commit lands on `add-usage-section`, not `main` |
| `git log --oneline --graph --all` | One line per commit, drawn as a graph, for ALL branches. `--graph` draws lines, `--all` includes every branch, not just the current one | See that the branch has one commit `main` does not |
| `git switch main` | Moves you back to `main`. Files in the folder change to match `main` | Prove branches are isolated |
| `cat README.md` | Prints the file | On `main` there is no Usage section |
| `git switch add-usage-section` | Moves back to the feature branch | The Usage section reappears |
| `cat README.md` | Prints the file again | Compare with the previous output |
| `git status -sb` | Short status for the current branch | Confirm clean state: `## add-usage-section` |

## What `git log --oneline --graph --all` output means
- `*` = a commit; the line of `*` shows history.
- `(HEAD -> add-usage-section)` = you are on this branch, at this commit.
- `(main)` = the `main` branch is at this commit.
- If the two labels are on different commits, the branch has moved ahead of `main`.

## Common mistakes in this exercise
- Running the `echo` lines before `git switch -c`: the edit is made on `main`. Fix: nothing is lost; `git switch -c` carries uncommitted edits to the new branch.
- Using `>` instead of `>>`: overwrites README.md. If it happens before committing, `git restore README.md` brings the old version back (and discards your overwrite).
- Committing while still on `main`: check `git branch` first.
