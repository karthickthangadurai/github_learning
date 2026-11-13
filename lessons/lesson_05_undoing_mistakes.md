# Lesson 5: Undoing Mistakes (`git restore`, `git reset`)

## Safety table

| Command | What it does | Safe? |
|---|---|---|
| `git restore --staged <file>` | Unstage, keep edits | Safe |
| `git reset --soft HEAD~1` | Undo last commit, keep changes staged | Safe |
| `git reset HEAD~1` (mixed, default) | Undo last commit, keep changes unstaged | Safe |
| `git restore <file>` | Discard unstaged edits in file | DESTRUCTIVE |
| `git reset --hard HEAD~1` | Undo last commit AND discard all changes | DESTRUCTIVE |

`HEAD~1` = one commit before the current one.

## Demo (throwaway repo)

A. Edited `config.txt` (temp 9.9), ran `git restore config.txt`: edit gone, file back to last commit.
B. Committed "Add top_p", ran `git reset --soft HEAD~1`: commit gone, change still staged.
C. Recommitted, ran `git reset --hard HEAD~1`: commit and change both gone.

## Exercise 5 (run in `github_learning`, safe only)

Do NOT use `restore` without `--staged` or `reset --hard` here.

```bash
echo "" >> README.md
echo "Temporary line." >> README.md
git commit -am "Temp commit"
git log --oneline
git reset --soft HEAD~1
git log --oneline
git status -s
git restore --staged README.md
git status -s
```
### Side note: `git commit -a` and `-am` (a trap we hit in this exercise)

- `-a` = stage **every modified tracked file**, then commit. It skips `git add`.
- `-m` = commit message. `-am` combines the two: `git commit -am "message"`.
- Tracked = Git already knows the file. `-a` never includes **untracked** (new) files.
- The trap: `-a` commits ALL modified tracked files, not just the one you were thinking about. In this exercise, `commit -am` also swept in `git_command_reference.md` and `progress_tracker.md`, not just `README.md`.
- Safer habit: `git status -s`, then `git add <file>` for each file you want, then `git commit -m "..."`. Use `-a` only when you've checked that every modified file belongs in the commit.

### What actually happened in the exercise

1. `commit -am` committed 3 files (README + the two notes), not 1.
2. `reset --soft HEAD~1` undid the commit but kept all 3 files **staged** (`M `).
3. That is why the two notes files showed `M ` and not just README.

Expected (corrected): the "Temp commit" disappears from the log. After `reset --soft`, all changed files stay staged. `restore --staged` only unstages the file you name.

The temporary README line is then removed with `git restore README.md` (destructive: it discards uncommitted edits, so check `git diff README.md` first).

Questions:
1. What is the difference between `reset --soft` and `reset --hard`?
2. Why is `git restore README.md` dangerous but `git restore --staged README.md` is not?

## Commands explained

| Command | What it does | Why you run it |
|---|---|---|
| `git commit -am "Temp commit"` | Stages ALL modified tracked files and commits. `-a` = all tracked changes, `-m` = message | A quick commit to then undo (careful: commits more than one file) |
| `git log --oneline` | Short history | See the commit before and after the reset |
| `git reset --soft HEAD~1` | Moves the branch back one commit; changes stay staged. `HEAD~1` = one commit before now | Undo the last commit and keep the work |
| `git status -s` | Short status | After reset, files show `M ` (staged) |
| `git restore --staged README.md` | Unstages the file, edits stay | Move from staged to unstaged |
| `git restore README.md` | DESTRUCTIVE: discards uncommitted edits in that file | Remove the temporary line (check `git diff` first) |
| `git reset --hard HEAD~1` | DESTRUCTIVE: undo the commit AND discard all changes | Only in throwaway repos or when sure |
