# Lesson 2: Your First Repository

**Scenario:** Starting a new AI project, `ai-notes`, with Git tracking from the first file.

## Demo steps (shown by Claude in a throwaway repo, now deleted; read only, nothing to run)

1. `git init -b main` creates the repo. A hidden `.git` folder appears (this is the whole history database). Output: `Initialized empty Git repository`.
2. `git status` shows `No commits yet` and `nothing to commit`.
3. Create `README.md`. `git status` lists it under **Untracked files** (Git sees it but doesn't track it).
4. `git add README.md` moves it to the staging area. `git status` now says **Changes to be committed: new file: README.md**.
5. `git commit -m "Add README"` saves the snapshot. Output: `[main (root-commit) 2477377] Add README`.
6. `git log` shows the commit: a unique hash, author, date, message.
7. `git status` now says **working tree clean**: working directory = last commit.

## Key ideas

- **Untracked** = new file Git has never recorded. **Staged** = queued for the next commit. **Committed** = saved in history.
- A commit is identified by a **hash** (the long hex string). `2477377` is its short form.
- `root-commit` means this is the first commit of the repo.
- "working tree clean" is the state you want before switching tasks.

## Exercise 2

You already ran `git init` and made your first commit (`Add README`) in `github_learning`. Now repeat the add/commit cycle on a second file, in the same folder:

```bash
git status
git add prompt.md
git status
git commit -m "Add learning prompt"
git log --oneline
```

Paste the terminal output, then answer:
1. What did `git status` say before and after `git add`, and why did it change?
2. Which of the three areas holds `prompt.md` after `git add`, and which after `git commit`?

New syntax: `git log --oneline` is a short version of `git log` that shows one line per commit (short hash + message).

## Recap

`init` → create file → `status` (untracked) → `add` (staged) → `commit` (saved) → `log`.
