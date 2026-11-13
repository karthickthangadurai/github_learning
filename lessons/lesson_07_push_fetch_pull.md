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
