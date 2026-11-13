# Lesson 7: Push, Fetch and Pull

## What you did
- Part A: committed lesson 6 notes and ran `git push`. Local `main` and `origin/main` matched.
- Part B: edited `README.md` on GitHub (a "teammate's" change), then:
  - `git fetch` -> `f43bbbc..1d5c4bd main -> origin/main`. Status became `[behind 1]`. Files unchanged.
  - `git pull` -> `Fast-forward`. Files updated, `[behind 1]` gone.

## Key ideas
- `origin/main` = your local record of the remote branch. Only fetch/pull/push update it.
- `[ahead N]` = you have N commits the remote lacks (need `git push`).
- `[behind N]` = remote has N commits you lack (need `git pull`).
- `fetch` = download only (safe, look first). `pull` = fetch + merge.
- Fast-forward = branch pointer just moves forward, no merge commit.
- Habit: `git pull` before you start work, `git push` when a unit of work is done.
- Do not rewrite or amend commits you have already pushed (e.g. the "lession" typo stays).

## Exercise 7 answers to check yourself
1. fetch only downloads; pull downloads and merges into your branch.
2. ahead = local commits not pushed; behind = remote commits not pulled.

## Commands used (in order)

### Part A: push a new commit
```bash
git add lessons/lesson_06_github_remote.md
git commit -m "Add lesson 6 notes"
git status -sb          # before push: ## main...origin/main [ahead 1]
git push
git status -sb          # after push:  ## main...origin/main
```
`-sb` = `-s` (short) + `-b` (show branch and ahead/behind info).

### Part B: simulate a teammate's change
1. On github.com, open `README.md`, click the pencil icon, add a line, commit it.
2. In the terminal:
```bash
git status -sb          # still looks in sync: Git hasn't looked at GitHub yet
git fetch               # download the new commit, files unchanged
git status -sb          # now: ## main...origin/main [behind 1]
git log --oneline origin/main     # see the remote's commits
git diff HEAD origin/main         # see what is different
git pull                # fetch + merge: files updated (Fast-forward)
git status -sb          # back to ## main...origin/main
cat README.md           # shows the line you added on GitHub
```

## Expected output highlights
- fetch: `f43bbbc..1d5c4bd  main -> origin/main`
- pull: `Fast-forward` and `README.md | 1 +`

## Common mistakes
- Expecting `git fetch` to change your files. It doesn't.
- Pulling with uncommitted edits to the same files. Commit or stash first.
- Fixing a typo in a pushed commit message by rewriting history. Don't, on shared branches.

## Recap
`push` uploads, `fetch` downloads safely, `pull` = fetch + merge. `[ahead]` = push needed, `[behind]` = pull needed.

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git add <file>` / `git commit -m "..."` | Stage and commit | Create the commit to push |
| `git status -sb` | Short status plus `ahead`/`behind` counts | See whether push or pull is needed |
| `git push` | Uploads local commits to GitHub | Share your commit |
| `git fetch` | Downloads new remote commits into `origin/main`. Your files are unchanged | See remote changes safely |
| `git log --oneline origin/main` | History of the remote-tracking branch | See what the remote has |
| `git diff HEAD origin/main` | Differences between your branch and the remote | Preview what a pull will change |
| `git pull` | `fetch` + merge into your branch | Bring your files up to date |
| `cat README.md` | Prints the file | Check the pulled change arrived |
