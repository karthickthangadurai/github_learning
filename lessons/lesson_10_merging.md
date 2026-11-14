# Lesson 10: Merging

**Scenario:** A feature branch is finished. Merging brings its commits into another branch (usually `main`).

## Start here: what "merge" means (plain language)

**Merge means copying the extra changes from your branch into `main`.** That's all it is.

Example with your own `README.md`:

Version on `main`:
```
Learning Git and GitHub step by step.
Edited on GitHub
```

Version on `add-usage-section` (two extra lines):
```
Learning Git and GitHub step by step.
Edited on GitHub

## Usage
Notes on practicing Git commands.
```

`main` is missing the Usage section because you made it on the branch. After you merge, `README.md` on `main` has the Usage section too.

Everyday analogy: `main` is a teacher's master copy of an exam. Your branch is a student's draft with corrections. Merge = the teacher accepts the corrections and writes them into the master copy.

Where you are in your repo (each letter is a commit):
```
main:               A --- B --- C
                                 \
add-usage-section:                D     <- your "Add usage section" commit
```
Merging brings `D` into `main`. Since `main` has not moved, Git just moves the `main` label forward to `D`. This is the "fast-forward" (see below).

Why two commands? Git needs to know two things:
1. Where do the changes GO? `git switch main` (stand on the receiving branch).
2. Where do the changes COME FROM? `git merge add-usage-section`.

`git merge add-usage-section` = "Take the changes from add-usage-section and put them into the branch I'm on."

Before-and-after check you can run:
```bash
git switch main
cat README.md                      # no Usage section
git merge add-usage-section
cat README.md                      # Usage section is there
```

## Key rule
You merge INTO the branch you are standing on. To put `feature` into `main`: `git switch main`, then `git merge feature`.

## Two kinds of merge

### 1. Fast-forward
`main` has not moved since the branch was created. Git just moves the `main` pointer forward. No merge commit. History stays a straight line.
```
before: main -> A      feature -> A-B
after:  main -> A-B (same as feature)
```

### 2. Merge commit (diverged)
Both branches have new commits. Git creates a new commit with TWO parents.
```
*   Merge branch 'feature-two'
|\
| * Add c on feature
* | Add d on main
|/
* Add b
```
If the same lines were changed on both sides, you get a merge CONFLICT (next lesson).

## Demo (throwaway repo)
```bash
git init -b main
echo "a" > a.txt && git add . && git commit -m "Add a"

# Case 1: fast-forward
git switch -c feature-one
echo "b" > b.txt && git add . && git commit -m "Add b"
git switch main
git merge feature-one                  # Fast-forward
git log --oneline --graph --decorate

# Case 2: diverged
git switch -c feature-two
echo "c" > c.txt && git add . && git commit -m "Add c on feature"
git switch main
echo "d" > d.txt && git add . && git commit -m "Add d on main"
git merge --no-edit feature-two        # Merge made by the 'ort' strategy
git log --oneline --graph --decorate

# Cleanup
git branch --merged
git branch -d feature-one feature-two
```

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git switch main` | Moves you onto `main` | Merge goes INTO the current branch |
| `git merge <branch>` | Brings `<branch>`'s commits into the current branch. Fast-forward if possible, merge commit otherwise | Combine finished work |
| `git merge --no-edit <branch>` | Same, but accepts the default merge message without opening an editor | Skip the editor |
| `git merge --no-ff <branch>` | Always create a merge commit, even if fast-forward is possible | Keep the branch visible in history |
| `git merge --abort` | Cancels a merge that hit conflicts, back to before the merge | Escape hatch |
| `git log --oneline --graph --decorate` | Draws history as a graph with branch labels (`--decorate` = show names) | See what the merge did |
| `git branch --merged` | Lists branches already merged into the current one | Find branches safe to delete |
| `git branch -d <name>` | Deletes a branch only if merged (safe). `-D` forces (destructive) | Clean up after merging |
| `git push origin --delete <name>` | Deletes a branch on GitHub | Clean up the remote |

## Common mistakes
- Merging in the wrong direction: you stand on the branch that RECEIVES the changes.
- Merging while you have uncommitted edits (commit or stash first).
- Forgetting `git pull` on `main` before merging, so `main` is out of date.
- Using `-D` and deleting unmerged work.

## Exercise 10 (in `github_learning`)
```bash
git status -sb
git switch main
git log --oneline --graph --all
git merge add-usage-section
git log --oneline --graph --all
cat README.md
git branch --merged
git branch -d add-usage-section
git branch
git push
git status -sb
```
Expected: `Fast-forward`; README on `main` now has the Usage section; branch deleted; `main` pushed.

Questions:
1. Why did Git say "Fast-forward" and not create a merge commit?
2. Which branch must you be on to merge `add-usage-section` into `main`?
3. Why does `git branch -d` refuse to delete an unmerged branch?
